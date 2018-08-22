---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Ajuste las modificaciones de base de datos dentro de una transacción (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial, aprenderá cómo permitir que las transacciones de base de datos...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: ab1ffa147545ab0d4fa0a3cce6f7dca91dfe3ffb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827999"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Ajustar las modificaciones de base de datos dentro de una transacción (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) o [descargar PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial, aprenderá cómo las transacciones de base de datos permiten modificaciones por lotes que se lleven a cabo como una operación atómica, lo que garantiza que todos los pasos se realizan correctamente o producirá un error en todos los pasos.


## <a name="introduction"></a>Introducción

Como hemos visto a partir de la [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, el control GridView ofrece compatibilidad integrada para la edición de nivel de fila y la eliminación. Con unos pocos clics del mouse es posible crear una interfaz de modificación de datos enriquecidos sin necesidad de escribir una línea de código, siempre y cuando está satisfecho con la edición y eliminación según una fila por fila. Sin embargo, en determinadas situaciones esto no es suficiente y necesitamos proporcionar a los usuarios la capacidad de editar o eliminar un lote de registros.

Por ejemplo, basada en web más clientes de correo electrónico usar una cuadrícula para mostrar todos los mensajes donde cada fila incluye una casilla de verificación junto con la información del correo electrónico s (asunto, remitente etc.). Esta interfaz permite al usuario eliminar varios mensajes Actívelas y haga clic en un botón Eliminar mensajes seleccionados. Un lote a la interfaz de edición es ideal en situaciones donde los usuarios normalmente Editar número de registros diferentes. En lugar de obligar al usuario hacer clic en Editar, realizar sus cambios y, a continuación, haga clic en Actualizar para cada registro que debe modificarse, cada fila con su interfaz de edición representa un lote en la interfaz de edición. El usuario puede modificar rápidamente el conjunto de filas que deben cambiarse y, a continuación, guardar estos cambios, haga clic en un botón Actualizar todo. En este conjunto de tutoriales, examinaremos cómo crear interfaces para insertar, editar y eliminar lotes de datos.

Al realizar operaciones por lotes se producirá un error importante determinar si debe ser posible para algunas de las operaciones del lote se realice correctamente mientras que otras. ¿Considere la posibilidad de un lote de eliminación de la interfaz: qué debe ocurrir si el primer registro seleccionado se elimina correctamente, pero el segundo se produce un error, por ejemplo, debido a una infracción de restricción de clave externa? ¿La eliminación de registro s. primera se debería deshacer o si es aceptable para el primer registro permanezca eliminados?

Si desea que la operación por lotes se trate como un [operación atómica](http://en.wikipedia.org/wiki/Atomic_operation), uno que ya sea todos los pasos se realizan correctamente o producirá un error en todos los pasos y, después, debe ampliarse para incluir compatibilidad con la capa de acceso a datos [base de datos las transacciones](http://en.wikipedia.org/wiki/Database_transaction). Las transacciones de base de datos garantizan la atomicidad para el conjunto de `INSERT`, `UPDATE`, y `DELETE` instrucciones se ejecutan bajo el paraguas de la transacción y son una característica admitida por la mayoría de los sistemas modernos de la base de datos de todos los.

En este tutorial, buscaremos en cómo ampliar la capa DAL para utilizar las transacciones de base de datos. Los tutoriales posteriores, examinarán implementación páginas web para insertar, actualizar y eliminar interfaces de lote. Introducción s Let!

> [!NOTE]
> Al modificar los datos en una transacción por lotes, no se necesita siempre atomicidad. En algunos escenarios, puede ser aceptable tener algunas modificaciones de datos se realice correctamente y otros usuarios en el mismo lote producirá un error, por ejemplo, cuando la eliminación de un conjunto de mensajes de correo electrónico desde un cliente de correo electrónico basado en web. Si hay s para procesar una mitad de error de base de datos a través de la eliminación, lo s probablemente aceptable que los registros procesados sin errores se conservan eliminados. En tales casos, la capa DAL no debe modificarse para admitir las transacciones de base de datos. Hay otros lotes operación escenarios, sin embargo, cuando atomicidad es vital. Cuando un cliente mueve sus fondos de una cuenta bancaria a otra, se deben realizar dos operaciones: los fondos deben ser la primera cuenta que se deducen y, a continuación, se agrega a la segunda. Mientras que el banco es posible que no cuenta con el primer paso se realice correctamente, pero producirá un error en el segundo paso, como es lógico sería malestares sus clientes. Lo animo a trabajar con este tutorial e implementar las mejoras de la capa DAL para admitir las transacciones de base de datos, incluso si no planea su uso en el lote de insertar, actualizar y eliminar interfaces que se va a compilar en los tres tutoriales siguientes.


## <a name="an-overview-of-transactions"></a>Información general de las transacciones

La mayoría de las bases de datos incluyen compatibilidad con *transacciones*, que permiten que varios comandos de base de datos que se desea agrupar en una sola unidad lógica de trabajo. Se garantiza que los comandos de base de datos que componen una transacción atómico, lo que significa que todos los comandos se producirá un error o todos se realizará correctamente.

En general, las transacciones se implementan a través de instrucciones SQL con el siguiente patrón:

1. Indicar el inicio de una transacción.
2. Ejecute las instrucciones SQL que componen la transacción.
3. Si se produce un error en una de las instrucciones del paso 2, revierta la transacción.
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Las instrucciones SQL usadas para crear, confirmar y revertir la transacción puede especificarse manualmente al escribir secuencias de comandos SQL o crear los procedimientos almacenados o mediante programación implica el uso de ADO.NET o las clases de la [ `System.Transactions` espacio de nombres](https://msdn.microsoft.com/library/system.transactions.aspx). En este tutorial, examinaremos solo administrar transacciones utilizando ADO.NET. En un futuro tutorial veremos cómo usar procedimientos almacenados en la capa de acceso a datos, momento en el que se explorará las instrucciones SQL para crear, revertir y confirmar las transacciones. Mientras tanto, consulte [administrar transacciones en procedimientos almacenados de SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obtener más información.

> [!NOTE]
> El [ `TransactionScope` clase](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) en el `System.Transactions` espacio de nombres permite a los desarrolladores ajustar mediante programación una serie de instrucciones dentro del ámbito de una transacción e incluye compatibilidad con transacciones complejas que implican varios orígenes, como dos bases de datos diferentes o incluso heterogéneos tipos de almacenes de datos, como una base de datos de Microsoft SQL Server, una base de datos de Oracle y un servicio Web. Se decidió usar transacciones de ADO.NET para este tutorial en lugar de quitar el `TransactionScope` clase porque es más específica para las transacciones de base de datos y, en muchos casos, ADO.NET es mucho menos consumen muchos recursos. Además, en ciertos escenarios el `TransactionScope` clase utiliza el Coordinador de transacciones distribuidas de Microsoft (MSDTC). Los problemas de rendimiento, implementación y configuración de MSDTC circundante facilita un tema avanzado y bastante especializado y más allá del ámbito de estos tutoriales.


Cuando se trabaja con el proveedor SqlClient en ADO.NET, las transacciones se inician a través de una llamada a la [ `SqlConnection` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que devuelve un [ `SqlTransaction` objeto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Las instrucciones de modificación de datos que la transacción de composición se colocan dentro de un `try...catch` bloque. Si se produce un error en una instrucción en el `try` bloquear las transferencias de ejecución a la `catch` bloque donde la transacción se puede revertir a través de la `SqlTransaction` objeto s [ `Rollback` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Si todas las instrucciones completa correctamente, una llamada a la `SqlTransaction` objeto s [ `Commit` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) al final de la `try` bloque confirma la transacción. El fragmento de código siguiente muestra este modelo. Consulte [mantener la coherencia de base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para una sintaxis adicional y ejemplos del uso de las transacciones con ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

De forma predeterminada, los TableAdapters del conjunto de datos con tipo no utiliza transacciones. Para proporcionar compatibilidad con las transacciones que se deba aumentar las clases TableAdapter para incluir métodos adicionales que usan el patrón anterior para realizar una serie de instrucciones de modificación de datos dentro del ámbito de una transacción. En el paso 2, veremos cómo usar las clases parciales para agregar estos métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Paso 1: Crear el trabajo con las páginas Web de datos por lotes

Antes de empezar a explorar cómo aumentar la capa DAL para admitir las transacciones de base de datos, permiten s primero dedique un momento para crear las páginas web ASP.NET que necesitamos para este tutorial y los tres siguientes. Empiece por agregar una carpeta nueva denominada `BatchData` y, a continuación, agregue las siguientes páginas ASP.NET, asociar cada página con el `Site.master` página maestra.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource


Al igual que con las demás carpetas `Default.aspx` usará el `SectionLevelTutorialListing.ascx` Control de usuario para enumerar los tutoriales dentro de su sección. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Por último, agregue estas cuatro páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la personalización del mapa del sitio `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para el trabajo con los tutoriales de datos por lotes.


![El mapa del sitio incluye ahora entradas para el trabajo con los tutoriales de datos por lotes](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: el mapa del sitio incluye ahora entradas para el trabajo con los tutoriales de datos por lotes


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Paso 2: Actualización de la capa de acceso a datos para admitir las transacciones de base de datos

Como se explicó en el primer tutorial, [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md), el conjunto de datos con tipo en nuestro DAL se compone de tablas de datos y TableAdapters. Las tablas de datos contienen datos mientras los TableAdapters proporcionan la funcionalidad para leer datos de la base de datos en las tablas de datos, para actualizar la base de datos con los cambios realizados en las tablas de datos y así sucesivamente. Recuerde que los TableAdapters proporcionan dos modelos para actualizar los datos, que conocen como actualización por lotes y directos de la base de datos. Con el patrón de actualización por lotes, lo TableAdapter se pasa un DataSet, DataTable o colección de objetos DataRow. Estos datos se enumera y para cada uno, insertar, modificar o Eliminar fila, el `InsertCommand`, `UpdateCommand`, o `DeleteCommand` se ejecuta. Con el patrón directos de la base de datos, lo TableAdapter se pasa en su lugar los valores de las columnas necesarias para insertar, actualizar o eliminar un registro único. El método de patrón directo de base de datos, a continuación, usa esos valores en el pasado para ejecutar adecuado `InsertCommand`, `UpdateCommand`, o `DeleteCommand` instrucción.

Usa el patrón de actualización, independientemente de los métodos generados automáticamente de los TableAdapters no utilizan transacciones. De forma predeterminada cada insert, update o delete realizadas por el TableAdapter se trata como una sola operación discreta. Por ejemplo, imagine que se usa el patrón directos de la base de datos por parte del código en el nivel de lógica empresarial para insertar los diez registros en la base de datos. Este código llamaría a TableAdapters `Insert` método diez veces. Si las cinco primeras inserciones se realizan correctamente, pero lo sexto dieron lugar a una excepción, los primeros cinco registros insertados permanecerán en la base de datos. De forma similar, si se usa el patrón de actualización por lotes para realizar inserciones, actualizaciones y eliminaciones a insertado, modificado y las filas eliminadas en una DataTable, si se ha realizado correctamente en el primer varias modificaciones, pero una posterior detectó un error, las modificaciones anteriores que completado permanecería en la base de datos.

En ciertos escenarios desea garantizar la atomicidad en toda una serie de modificaciones. Para lograr esto debemos manualmente ampliamos el TableAdapter mediante la adición de nuevos métodos que se ejecutan los `InsertCommand`, `UpdateCommand`, y `DeleteCommand` s bajo el paraguas de una transacción. En [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) contemplamos utilizando [clases parciales](http://en.wikipedia.org/wiki/Partial_type) para ampliar la funcionalidad de las DataTables del conjunto de datos con tipo. Esta técnica también puede usarse con los TableAdapters.

El conjunto de datos con tipo `Northwind.xsd` se encuentra en la `App_Code` carpeta s `DAL` subcarpeta. Cree una subcarpeta en el `DAL` carpeta denominada `TransactionSupport` y agregue un nuevo archivo de clase denominado `ProductsTableAdapter.TransactionSupport.cs` (consulte la figura 4). Este archivo contendrá la implementación parcial de la `ProductsTableAdapter` que incluye métodos para realizar las modificaciones de datos mediante una transacción.


![Agregar una carpeta denominada el TransactionSupport y un archivo de clase denominado ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: agregue una carpeta denominada `TransactionSupport` y el archivo de clase `ProductsTableAdapter.TransactionSupport.cs`


Escriba el código siguiente en el `ProductsTableAdapter.TransactionSupport.cs` archivo:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

El `partial` palabra clave en la declaración de clase aquí indica al compilador que son los miembros agregados en que se agregarán a la `ProductsTableAdapter` clase en el `NorthwindTableAdapters` espacio de nombres. Tenga en cuenta el `using System.Data.SqlClient` instrucción en la parte superior del archivo. Dado que el TableAdapter se configuró para usar el proveedor SqlClient, internamente se usa un [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objeto para emitir comandos a la base de datos. Por lo tanto, debemos usar la `SqlTransaction` clase para iniciar la transacción y, después, confirmarla o revertirla. Si usa un almacén de datos que no sean de Microsoft SQL Server, deberá utilizar el proveedor adecuado.

Estos métodos proporcionan los bloques de creación necesarios para iniciar, reversión y confirmación una transacción. Se marcan `public`, lo que les permite que se usará desde el `ProductsTableAdapter`, de otra clase en la capa DAL, o de otra capa en la arquitectura, como la capa BLL. `BeginTransaction` Abre el TableAdapter interno `SqlConnection` (si es necesario), comienza la transacción y lo asigna a la `Transaction` propiedad y se asocia la transacción a interno `SqlDataAdapter` s `SqlCommand` objetos. `CommitTransaction` y `RollbackTransaction` llamar a la `Transaction` objeto s `Commit` y `Rollback` métodos, respectivamente, antes de cerrar interno `Connection` objeto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Paso 3: Agregar métodos para actualizar y eliminar datos bajo el paraguas de una transacción

Con estos métodos completados, hemos re está listo para agregar métodos a `ProductsDataTable` o BLL que llevan a cabo una serie de comandos bajo el paraguas de una transacción. El método siguiente utiliza el patrón de actualización por lotes para actualizar un `ProductsDataTable` instancia mediante una transacción. Inicia una transacción mediante una llamada a la `BeginTransaction` método y, a continuación, usa un `try...catch` bloque para emitir las instrucciones de modificación de datos. Si la llamada a la `Adapter` objeto s `Update` método produce una excepción, la ejecución se transferirá a la `catch` bloque donde se revertirá la transacción y la excepción que se vuelvan a lanzar. Recuerde que el `Update` método implementa el patrón de actualización por lotes mediante la enumeración de las filas de la `ProductsDataTable` y la realización de la necesaria `InsertCommand`, `UpdateCommand`, y `DeleteCommand` s. Si cualquiera de estos resultados de comandos en un error, la transacción se revierte, deshace los cambios realizados durante la vigencia de s de transacciones en el anteriores. Debe el `Update` instrucción completará sin errores, la transacción se confirma en su totalidad.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Agregar el `UpdateWithTransaction` método a la `ProductsTableAdapter` clase a través de la clase parcial en `ProductsTableAdapter.TransactionSupport.cs`. Como alternativa, este método se puede agregar a la capa de lógica empresarial de s `ProductsBLL` clase con algunos cambios menores de sintaxis. Es decir, la palabra clave en `this.BeginTransaction()`, `this.CommitTransaction()`, y `this.RollbackTransaction()` tendría que reemplazarse con `Adapter` (Recuerde que `Adapter` es el nombre de una propiedad en `ProductsBLL` de tipo `ProductsTableAdapter`).

El `UpdateWithTransaction` método usa el patrón de actualización por lotes, pero también se puede usar una serie de llamadas directas de la base de datos dentro del ámbito de una transacción, como se muestra en el siguiente método. El `DeleteProductsWithTransaction` método acepta como entrada un `List<T>` typu `int`, que son el `ProductID` que se eliminan. El método inicia la transacción a través de una llamada a `BeginTransaction` y, a continuación, en el `try` en bloques, recorre en iteración la lista proporcionada llamar el patrón de DB-Direct `Delete` método para cada `ProductID` valor. Si cualquiera de las llamadas a `Delete` se produce un error, el control se transfiere a la `catch` bloque donde se revierte la transacción y la excepción que se vuelvan a lanzar. Si todas las llamadas a `Delete` se realiza correctamente, a continuación, se confirma la transacción. Agregue este método para el `ProductsBLL` clase.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicación de transacciones a través de varios de los TableAdapters

El código relacionado con la transacción examinado en este tutorial permite varias instrucciones en el `ProductsTableAdapter` se traten como una operación atómica. Pero ¿qué ocurre si deben realizarse de forma atómica varias modificaciones a las tablas de base de datos diferente? Por ejemplo, al eliminar una categoría, podríamos primero queremos volver a asignar sus productos actuales a otra categoría. Estos dos pasos reasignar los productos y la eliminación de la categoría deben ejecutarse como una operación atómica. Pero la `ProductsTableAdapter` incluye solo los métodos para modificar el `Products` tabla y el `CategoriesTableAdapter` incluye solo los métodos para modificar el `Categories` tabla. ¿Cómo una transacción puede abarcar ambos TableAdapters?

Una opción consiste en Agregar un método para el `CategoriesTableAdapter` denominado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` y que ese método llame a un procedimiento almacenado que reasigna los productos y elimina la categoría dentro del ámbito de una transacción definida dentro del procedimiento almacenado. Veremos cómo iniciar, confirmar y deshacer las transacciones en procedimientos almacenados en un futuro tutorial.

Otra opción consiste en crear una clase auxiliar en la capa DAL que contiene el `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` método. Este método crearía una instancia de la `CategoriesTableAdapter` y `ProductsTableAdapter` y, a continuación, establezca estos dos TableAdapters `Connection` propiedades al mismo `SqlConnection` instancia. En ese punto, uno de los dos TableAdapters iniciaría la transacción con una llamada a `BeginTransaction`. Sería necesario invocar los métodos de TableAdapter para reasignar los productos y eliminar la categoría en un `try...catch` bloque con la transacción que se confirmarán o revertirán volver según sea necesario.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Paso 4: Agregar el`UpdateWithTransaction`método a la capa de lógica de negocios

En el paso 3 se ha agregado un `UpdateWithTransaction` método a la `ProductsTableAdapter` en la capa DAL. Debemos agregar un método correspondiente a la capa BLL. Aunque la capa de presentación podría llamar directamente a la capa DAL para invocar el `UpdateWithTransaction` método, han luchado por estos tutoriales definir una arquitectura por capas que aísla la capa DAL desde la capa de presentación. Por lo tanto, corresponde a nosotros para seguir este enfoque.

Abra el `ProductsBLL` archivo de clase y agregue un método denominado `UpdateWithTransaction` que llama simplemente hasta el método correspondiente de la capa DAL. Ahora debería haber dos nuevos métodos en `ProductsBLL`: `UpdateWithTransaction`, que acaba de agregar, y `DeleteProductsWithTransaction`, que se agregó en el paso 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Estos métodos no incluyen el `DataObjectMethodAttribute` atributo asignado a la mayoría de los otra métodos en el `ProductsBLL` clase porque se invocará estos métodos directamente desde las clases de código subyacente de las páginas ASP.NET. Recuerde que `DataObjectMethodAttribute` se usa para marcar los métodos que deben aparecer en la s ObjectDataSource Configurar origen de datos asistente y en qué ficha (SELECT, UPDATE, INSERT o DELETE). Puesto que el control GridView no tiene ninguna compatibilidad integrada para batch editen o eliminen, tenemos invocar estos métodos mediante programación, en lugar de utilizar el enfoque sin código declarativo.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Paso 5: Actualización de forma atómica bases de datos de la capa de presentación

Para ilustrar el efecto que la transacción tiene al actualizar un lote de registros, s permiten crear una interfaz de usuario que se enumera todos los productos en un control GridView e incluye un sitio Web de botón de control que, al hacer clic, reasigna los productos `CategoryID` valores. En concreto, la reasignación de categoría progresan para que varios productos de la primera se asignan válido `CategoryID` valor mientras que otros son útiles asigna un inexistente `CategoryID` valor. Si se intenta actualizar la base de datos con un producto cuyo `CategoryID` no coincide con una categoría existente s `CategoryID`, se producirá una infracción de restricción de clave externa y se producirá una excepción. Lo veremos en este ejemplo es que cuando el uso de una transacción de la excepción de la infracción de restricción de clave externa hará que el anterior válida `CategoryID` los cambios se desharán. Si no usa una transacción, sin embargo, seguirá siendo las modificaciones en las categorías iniciales.

Comience abriendo la `Transactions.aspx` página en el `BatchData` carpetas y arrastre un control GridView del cuadro de herramientas hasta el diseñador. Establezca su `ID` a `Products` y, en la etiqueta inteligente, de enlazarla a un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para extraer sus datos de la `ProductsBLL` clase s `GetProducts` método. Esto se un control GridView de solo lectura, por lo que establece las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None) y haga clic en Finalizar.


[![Figura 5: Configurar el origen ObjectDataSource para usar el método de clase ProductsBLL s GetProducts](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: figura 5: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase s `GetProducts` método ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y un CampoCasillaVerificación para los campos de datos del producto. Quite todos estos campos, excepto para `ProductID`, `ProductName`, `CategoryID`, y `CategoryName` y cambiar el nombre de la `ProductName` y `CategoryName` BoundFields `HeaderText` propiedades para el producto y categoría, respectivamente. En la etiqueta inteligente, active la opción de habilitar la paginación. Después de realizar estas modificaciones, el marcado declarativo de s GridView y ObjectDataSource debería ser similar al siguiente:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

A continuación, agregue tres controles de botón Web por encima del control GridView. Establecer el primer botón s propiedad Text para actualizar la cuadrícula, la segunda s para modificar categorías (con la transacción) y el tercero s para modificar categorías (sin transacción).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

En este momento, la vista de diseño en Visual Studio debe ser similar a la que se muestra en la figura 7 de captura de pantalla.


[![La página contiene tres controles Button de Web y de un control GridView](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: la página contiene un control GridView y tres controles Button de Web ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Crear controladores de eventos para cada uno de los tres botón s `Click` eventos y use el código siguiente:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

La actualización de botón s `Click` controlador de eventos simplemente vuelve a enlazar los datos en GridView mediante una llamada a la `Products` GridView s `DataBind` método.

El segundo controlador de eventos reasigna los productos `CategoryID` s y se usa el nuevo método de transacción de la capa BLL para realizar la base de datos actualiza bajo el paraguas de una transacción. Tenga en cuenta que cada producto s `CategoryID` se establece arbitrariamente en el mismo valor que su `ProductID`. Esto funcionará correctamente para el primer algunos productos, dado que esos productos tienen `ProductID` valores que se producen asignar a válido `CategoryID` s. Pero una vez el `ProductID` inicio s demasiado grande, esta superposición una coincidencia de `ProductID` s y `CategoryID` s ya no es aplicable.

La tercera `Click` controlador de eventos actualiza los productos `CategoryID` s de la misma manera, pero envía la actualización a la base de datos mediante el `ProductsTableAdapter` predeterminados `Update` método. Esto `Update` método no se ajusta a la serie de comandos dentro de una transacción, se mantendrá por lo que dichos cambios se realizan antes que el primer error de infracción de restricción de clave externa se ha encontrado.

Para demostrar este comportamiento, visite esta página a través de un explorador. Inicialmente verá la primera página de datos tal como se muestra en la figura 8. A continuación, haga clic en el botón Modificar categorías (con la transacción). Esto se producen un postback y error al intentar actualizar todos los productos `CategoryID` valores, pero se producirá una infracción de restricción de clave externa (consulte la figura 9).


[![Los productos se muestran en un control GridView paginable](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: los productos se muestran en un control GridView paginable ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Reasignación de los resultados de las categorías en una infracción de restricción de clave externa](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: reasignar los resultados de las categorías en una infracción de restricción de clave externa ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Ahora presione el botón Atrás de explorador s y, a continuación, haga clic en el botón Actualizar la cuadrícula. Al actualizar los datos debería ver el mismo resultado exacto tal como se muestra en la figura 8. Es decir, incluso aunque algunos de los productos `CategoryID` s eran los valores modificados a legal y actualiza en la base de datos, se revirtieron cuando se produjo la infracción de restricción de clave externa.

Ahora intente hacer clic en el botón Modificar categorías (sin transacción). Esto dará como resultado el mismo error de infracción de restricción de clave externa (consulte la figura 9), pero esta vez aquellos productos cuyo `CategoryID` los valores han cambiado a un legal valor no se revertirán. Presione el botón Atrás de explorador s y, a continuación, el botón de actualización de la cuadrícula. Como se muestra en la figura 10, la `CategoryID` reasignación s de los ocho primeros productos. Por ejemplo, en la figura 8, Chang tenía un `CategoryID` de 1, pero en la figura 10 TI s se ha reasignado a 2.


[![Algunos valores de Id. de categoría de productos actualizado mientras otros se han no estaban](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: algunos productos `CategoryID` valores actualizados mientras otros se han no eran ([haga clic aquí para ver imagen en tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Resumen

De forma predeterminada, los métodos de TableAdapter s no incluyen las instrucciones ejecutadas de la base de datos dentro del ámbito de una transacción, pero con un poco de trabajo podemos agregar métodos que va a crear, confirmar y revertir una transacción. En este tutorial, hemos creado tres tales métodos en el `ProductsTableAdapter` clase: `BeginTransaction`, `CommitTransaction`, y `RollbackTransaction`. Hemos visto cómo usar estos métodos junto con un `try...catch` bloque para realizar una serie de instrucciones de modificación de datos atómico. En concreto, hemos creado la `UpdateWithTransaction` método en el `ProductsTableAdapter`, que usa el patrón de actualización por lotes para realizar las modificaciones necesarias para las filas de una proporcionado `ProductsDataTable`. También hemos agregado la `DeleteProductsWithTransaction` método a la `ProductsBLL` clase en el BLL, que acepta un `List` de `ProductID` valores como entrada y llama al método de patrón de DB-Direct `Delete` para cada `ProductID`. Ambos métodos empiece por crear una transacción y, a continuación, ejecutar las instrucciones de modificación de datos dentro de un `try...catch` bloque. Si se produce una excepción, se revierte la transacción, en caso contrario, se confirma.

Paso 5 muestra el efecto de las actualizaciones de lote transaccional frente a las actualizaciones por lotes que dejó de usar una transacción. En los tres tutoriales siguientes se compilará en los fundamentos establecidos en este tutorial y crear interfaces de usuario para realizar inserciones, eliminaciones y actualizaciones por lotes.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Mantener la coherencia de base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Procedimientos almacenados de administración de transacciones en SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transacciones realizadas fácil: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope y objetos DataAdapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Uso de transacciones de bases de datos de Oracle en .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Dave Gardner, Hilton Giesenow y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](batch-updating-cs.md)
