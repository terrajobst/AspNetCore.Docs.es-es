---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: "Ajuste las modificaciones de base de datos dentro de una transacción (C#) | Documentos de Microsoft"
author: rick-anderson
description: "Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial se obtenga información acerca de cómo permitir que las transacciones de base de datos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: e2dafdf9a9414bddfca37ef942856c94096f35b8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Ajuste las modificaciones de base de datos dentro de una transacción (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) o [descarga de PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial se obtenga información acerca de cómo permitir las transacciones de base de datos que las modificaciones de lote se lleve a cabo como una operación atómica, lo que garantiza que todos los pasos se realizan correctamente o producirá un error en todos los pasos.


## <a name="introduction"></a>Introducción

Como hemos visto a partir de la [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView proporciona compatibilidad integrada para la edición de nivel de fila y eliminar. Con unos pocos clics del mouse es posible crear una interfaz de modificación de datos enriquecidos sin escribir una línea de código, siempre y cuando el contenido con la edición y eliminación por fila. Sin embargo, en ciertos escenarios esto es suficiente y es necesario proporcionar a los usuarios la capacidad de modificar o eliminar un lote de registros.

Por ejemplo, basada en web más clientes de correo electrónico utilizar una cuadrícula para mostrar cada mensaje donde cada fila incluye una casilla junto con la información de s de correo electrónico (asunto, remitente y así sucesivamente). Esta interfaz permite al usuario eliminar varios mensajes, comprobando que dichos datos y, a continuación, haga clic en un botón Eliminar mensajes seleccionados. Un lote a la interfaz de edición es ideal en situaciones donde los usuarios normalmente editar varios registros diferentes. En lugar de obligarle a los usuarios hacer clic en Editar, realizar sus cambios y, a continuación, haga clic en Actualizar para cada registro que se deba modificarse, representa como un lote en la interfaz de edición cada fila a su interfaz de edición. El usuario puede modificar rápidamente el conjunto de filas que deben cambiarse y, a continuación, guardar estos cambios, haga clic en un botón Actualizar todo. En este conjunto de tutoriales examinaremos cómo crear interfaces para insertar, modificar y eliminar lotes de datos.

Al realizar operaciones por lotes se producirá un error importante determinar si debe ser posible para algunas de las operaciones del lote sea correcta mientras que otras. ¿Considere la posibilidad de un lote al eliminar la interfaz - qué debe ocurrir si el primer registro seleccionado se elimina correctamente, pero la segunda se produce un error, por ejemplo, debido a una infracción de restricción de clave externa? ¿La eliminación de registro s primera se debería deshacer o es aceptable para el primer registro permanezca eliminadas?

Si desea que la operación por lotes se trate como un [operación atómica](http://en.wikipedia.org/wiki/Atomic_operation), una donde ya sea todos los pasos correctamente o producirá un error en todos los pasos, debe aumentar para incluir compatibilidad con la capa de acceso a datos [base de datos las transacciones](http://en.wikipedia.org/wiki/Database_transaction). Las transacciones de base de datos garantizan la atomicidad para el conjunto de `INSERT`, `UPDATE`, y `DELETE` instrucciones ejecutado bajo el paraguas de la transacción y son una característica compatible con la mayoría todos los sistemas de base de datos modernas.

En este tutorial se examinará cómo ampliar la capa DAL para utilizar las transacciones de base de datos. Los tutoriales posteriores examinará implementación páginas web para insertar, actualizar y eliminar interfaces de lote. Permiten s empiece a trabajar.

> [!NOTE]
> Al modificar datos en una transacción por lotes, no se necesita siempre atomicidad. En algunos escenarios, puede ser aceptable disponer de algunas modificaciones de datos correctamente y otros usuarios en el mismo lote producirá un error, como cuando la eliminación de un conjunto de mensajes de correo electrónico desde un cliente de correo electrónico basado en web. Si hay s para procesar una mitad del error de base de datos a través de la eliminación, s probablemente aceptable que esos registros procesados sin errores quedan eliminados. En esos casos, la capa DAL no necesita modificarse para admitir las transacciones de base de datos. Hay otros lote operación escenarios, sin embargo, donde la atomicidad es vital. Cuando un cliente mueve sus fondos de una cuenta bancaria a otra, se deben realizar dos operaciones: los fondos se deben deducir de la primera cuenta y, a continuación, se agrega a la segunda. Mientras el banco no posible problema con el primer paso correctamente pero se producirá un error en el segundo paso, como es lógico sería abrumados sus clientes. Le animo a trabajar en este tutorial e implementar las mejoras de la capa DAL para admitir las transacciones de base de datos, incluso si no tiene previsto en su uso en el lote de inserción, actualización y eliminación interfaces que desarrollaremos en los tres tutoriales siguientes.


## <a name="an-overview-of-transactions"></a>Información general de las transacciones

La mayoría de las bases de datos incluyen compatibilidad con *transacciones*, que permiten que varios comandos de base de datos que se desea agrupar en una sola unidad lógica de trabajo. Se garantiza que los comandos de base de datos que componen una transacción atómica, lo que significa que todos los comandos se producirá un error o todos se realizará correctamente.

En general, las transacciones se implementan a través de instrucciones SQL utilizando el modelo siguiente:

1. Indicar el inicio de una transacción.
2. Ejecute las instrucciones SQL que componen la transacción.
3. Si se produce un error en una de las instrucciones del paso 2, revertir la transacción.
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Las instrucciones SQL usadas para crear, confirmar y revertir la transacción se puede introducir manualmente al escribir secuencias de comandos SQL o crear procedimientos almacenados o mediante programación implicará la utilización de ADO.NET o las clases de la [ `System.Transactions` espacio de nombres](https://msdn.microsoft.com/library/system.transactions.aspx). En este tutorial que sólo examinaremos administrar transacciones utilizando ADO.NET. En un tutorial posterior, veremos cómo usar procedimientos almacenados en la capa de acceso a datos, momento en el que se explorará las instrucciones SQL para crear, revertir y confirmar las transacciones. Mientras tanto, consulte [administrar transacciones en procedimientos almacenados de SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obtener más información.

> [!NOTE]
> El [ `TransactionScope` clase](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) en el `System.Transactions` espacio de nombres permite a los desarrolladores ajustar mediante programación una serie de instrucciones dentro del ámbito de una transacción e incluye compatibilidad para las transacciones complejas que requieren varios orígenes, como dos bases de datos diferentes o incluso heterogéneos tipos de almacenes de datos, como una base de datos de Microsoft SQL Server y una base de datos de Oracle, un servicio Web. ¿Ha decidido utilizar transacciones de ADO.NET para este tutorial, en lugar de la `TransactionScope` clase porque ADO.NET es más específica para las transacciones de base de datos y, en muchos casos, es mucho menos recursos de forma intensiva. Además, en ciertos escenarios el `TransactionScope` clase utiliza el Coordinador de transacciones distribuidas de Microsoft (MSDTC). Los problemas de configuración, implementación y rendimiento MSDTC adyacente facilita un tema en su lugar especializado y avanzado y fuera del ámbito de estos tutoriales.


Cuando se trabaja con el proveedor SqlClient en ADO.NET, las transacciones se inician a través de una llamada a la [ `SqlConnection` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que devuelve un [ `SqlTransaction` objeto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Las instrucciones de modificación de datos que la transacción de creación se colocan dentro de un `try...catch` bloque. Si se produce un error en una instrucción en el `try` bloquear las transferencias de ejecución a la `catch` bloque donde la transacción puede revertirse a través de la `SqlTransaction` objeto s [ `Rollback` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Si todas las instrucciones completen correctamente, una llamada a la `SqlTransaction` objeto s [ `Commit` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) al final de la `try` bloque confirma la transacción. El fragmento de código siguiente muestra este patrón. Vea [mantener la coherencia de base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para adicionales de la sintaxis y ejemplos de uso de transacciones con ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

De forma predeterminada, los TableAdapters en un conjunto de datos con tipo no utiliza transacciones. Para proporcionar compatibilidad con transacciones que es necesario para aumentar las clases TableAdapter para incluir métodos adicionales que usan el patrón anterior para realizar una serie de instrucciones de modificación de datos dentro del ámbito de una transacción. En el paso 2, veremos cómo usar las clases parciales para agregar estos métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Paso 1: Crear el trabajo con páginas Web de datos por lotes

Antes de empezar a explorar cómo aumentar la capa DAL para admitir las transacciones de base de datos, permiten s primero Tómese un momento para crear las páginas web ASP.NET que necesitamos para este tutorial y los tres siguientes. Empiece por agregar una nueva carpeta denominada `BatchData` y, a continuación, agregue las siguientes páginas ASP.NET, asociar cada página con el `Site.master` página maestra.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con SqlDataSource


Al igual que con las otras carpetas, `Default.aspx` utilizará el `SectionLevelTutorialListing.ascx` Control de usuario para mostrar los tutoriales dentro de su sección. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página s la vista Diseño.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Por último, agregue estos cuatro páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la personalización del mapa del sitio `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para el trabajo con los tutoriales de datos por lotes.


![El mapa del sitio ahora incluye las entradas para el trabajo con los tutoriales de datos por lotes](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: el mapa del sitio ahora incluye las entradas para el trabajo con los tutoriales de datos por lotes


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Paso 2: Actualizar la capa de acceso a datos para admitir las transacciones de base de datos

Como se explicó en el primer tutorial, [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md), el conjunto de datos con tipo en la capa DAL se compone de tablas de datos y TableAdapters. Las tablas de datos contienen datos, mientras que los TableAdapters proporcionan la funcionalidad necesaria para leer datos de la base de datos en las tablas de datos, para actualizar la base de datos con los cambios realizados en las tablas de datos y así sucesivamente. Recuerde que los TableAdapters proporcionan dos modelos para actualizar datos, que conocen como actualización por lotes y directo de la base de datos. Con el patrón de actualización por lotes, lo TableAdapter se pasa un conjunto de datos, un objeto DataTable o una colección de filas de datos. Estos datos se enumera y para cada insertadas, modificar o eliminar la fila, el `InsertCommand`, `UpdateCommand`, o `DeleteCommand` se ejecuta. Con el patrón de DB-Direct, lo TableAdapter se pasa en su lugar, los valores de las columnas necesarias para insertar, actualizar o eliminar un único registro. El método de patrón directo de base de datos, a continuación, utiliza los valores pasados en para ejecutar adecuado `InsertCommand`, `UpdateCommand`, o `DeleteCommand` instrucción.

Sin tener en cuenta la utiliza el patrón de actualización, los métodos generados automáticamente de los TableAdapters no utilizan transacciones. De forma predeterminada cada insert, update o delete realizadas por el TableAdapter se trata como una única operación discreta. Por ejemplo, imagine que se usa el patrón de DB-Direct por parte del código en la capa BLL para insertar diez registros en la base de datos. Este código llamaría a TableAdapters `Insert` método diez veces. Si las cinco primeras inserciones correctamente, pero el sexto uno dio como resultado una excepción, se mantendría los cinco primeros registros insertados en la base de datos. De forma similar, si el patrón de actualización por lotes se utiliza para realizar inserciones, actualizaciones y eliminaciones a las filas insertadas, modificar y eliminar filas en un objeto DataTable, si el primer algunas modificaciones se realizó correctamente, pero una posterior encontró un error, las modificaciones anteriores que completado permanecerá en la base de datos.

En determinados escenarios que queremos garantizar la atomicidad de una serie de modificaciones. Para lograr esto debemos manualmente extendemos TableAdapter mediante la adición de nuevos métodos que se ejecutan los `InsertCommand`, `UpdateCommand`, y `DeleteCommand` s bajo el paraguas de una transacción. En [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) analizamos utilizando [clases parciales](http://en.wikipedia.org/wiki/Partial_type) para ampliar la funcionalidad de las tablas de datos en el conjunto de datos con tipo. Esta técnica también puede usarse con TableAdapters.

El conjunto de datos con tipo `Northwind.xsd` se encuentra en la `App_Code` carpeta s `DAL` subcarpeta. Crear una subcarpeta de la `DAL` carpeta denominada `TransactionSupport` y agregue un nuevo archivo de clase denominado `ProductsTableAdapter.TransactionSupport.cs` (consulte la figura 4). Este archivo contendrá la implementación parcial de la `ProductsTableAdapter` que incluye métodos para realizar las modificaciones de datos utilizando una transacción.


![Agregar una carpeta denominada TransactionSupport y un archivo de clase denominado ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: agregar una carpeta denominada `TransactionSupport` y un archivo de clase denominado`ProductsTableAdapter.TransactionSupport.cs`


Escriba el código siguiente en el `ProductsTableAdapter.TransactionSupport.cs` archivo:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

El `partial` palabra clave en la declaración de clase aquí se indica al compilador que va a agregar a los miembros agregados el `ProductsTableAdapter` clase en el `NorthwindTableAdapters` espacio de nombres. Tenga en cuenta el `using System.Data.SqlClient` instrucción en la parte superior del archivo. Dado que el TableAdapter se configuró para usar el proveedor SqlClient, internamente se usa un [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objeto que se va a emitir sus comandos a la base de datos. Por lo tanto, es necesario utilizar la `SqlTransaction` clase para iniciar la transacción y, a continuación, para confirmarla o revertirla. Si está utilizando un almacén de datos que no sea de Microsoft SQL Server, debe utilizar el proveedor adecuado.

Estos métodos ofrecen los bloques de creación necesarios para iniciar, reversión y confirmación una transacción. Se marcan `public`, les permite usar desde lo que el `ProductsTableAdapter`, de otra clase en la capa DAL, o desde otra capa de la arquitectura, como la capa BLL. `BeginTransaction`Abre el TableAdapter interno `SqlConnection` (si es necesario), comienza la transacción y lo asigna a la `Transaction` propiedad y asocia la transacción con interno `SqlDataAdapter` s `SqlCommand` objetos. `CommitTransaction`y `RollbackTransaction` llamar a la `Transaction` objeto s `Commit` y `Rollback` métodos, respectivamente, antes de cerrar interno `Connection` objeto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Paso 3: Agregar métodos para actualizar y eliminar datos bajo el paraguas de una transacción

Con estos métodos completados, se re está listo para agregar métodos a `ProductsDataTable` o BLL que llevan a cabo una serie de comandos bajo el paraguas de una transacción. El siguiente método usa el patrón de actualización por lotes para actualizar un `ProductsDataTable` instancia mediante una transacción. Inicia una transacción mediante una llamada a la `BeginTransaction` método y, a continuación, utiliza un `try...catch` bloque para emitir las instrucciones de modificación de datos. Si la llamada a la `Adapter` objeto s `Update` método produce una excepción, la ejecución se transferirá a la `catch` bloque donde la transacción se revertirá y la excepción que se vuelve a producir. Recuerde que el `Update` método implementa el patrón de actualización por lotes con una enumeración de las filas de proporcionado `ProductsDataTable` y realizar las necesarias `InsertCommand`, `UpdateCommand`, y `DeleteCommand` s. Si cualquiera de estos resultados de comandos en un error, la transacción se revierte, deshaciendo las modificaciones previas realizadas durante la duración de s de la transacción. Debe el `Update` instrucción se completan sin error, la transacción se confirma en su totalidad.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Agregar el `UpdateWithTransaction` método a la `ProductsTableAdapter` clase a través de la clase parcial en `ProductsTableAdapter.TransactionSupport.cs`. Como alternativa, este método podría agregarse a la capa de lógica empresarial s `ProductsBLL` clase con unos pequeños cambios sintácticos. Es decir, la palabra clave en `this.BeginTransaction()`, `this.CommitTransaction()`, y `this.RollbackTransaction()` tendría que ser reemplazados `Adapter` (Recuerde que `Adapter` es el nombre de una propiedad en `ProductsBLL` de tipo `ProductsTableAdapter`).

El `UpdateWithTransaction` método usa el patrón de actualización por lotes, pero también se puede usar una serie de llamadas de DB-Direct dentro del ámbito de una transacción, como se muestra en el siguiente método. El `DeleteProductsWithTransaction` método acepta como entrada un `List<T>` de tipo `int`, ¿cuáles son los `ProductID` que se eliminan. El método inicia la transacción mediante una llamada a `BeginTransaction` y, a continuación, en la `try` bloquear, recorre en iteración la lista proporcionada al llamar al patrón de DB-Direct `Delete` método para cada `ProductID` valor. Si alguna de las llamadas a `Delete` se produce un error, el control se transfiere a la `catch` bloque donde se revierte la transacción y la excepción que se vuelve a producir. Si todas las llamadas a `Delete` se realiza correctamente, se confirma la transacción. Agregue este método a la `ProductsBLL` clase.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicar las transacciones entre varios TableAdapters

Permite que el código relacionado con la transacción que se examinan en este tutorial para varias instrucciones en el `ProductsTableAdapter` se trate como una operación atómica. Pero ¿qué ocurre si deben realizarse de forma atómica varias modificaciones a las tablas de base de datos diferente? Por ejemplo, cuando se elimina una categoría, nos convenga primero para volver a asignar sus productos actuales a otra categoría. Estos dos pasos reasignar los productos y eliminar la categoría deben ejecutarse como una operación atómica. Pero la `ProductsTableAdapter` incluye solo los métodos para modificar el `Products` tabla y la `CategoriesTableAdapter` incluye solo los métodos para modificar el `Categories` tabla. Entonces, ¿cómo puede incluir una transacción ambos TableAdapters?

Una opción consiste en Agregar un método a la `CategoriesTableAdapter` denominado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` y ese método llame a un procedimiento almacenado que se reasigna los productos y elimina la categoría dentro del ámbito de una transacción definida dentro del procedimiento almacenado. Analizaremos cómo iniciar, confirmar y deshacer las transacciones en procedimientos almacenados en un tutorial posterior.

Otra opción consiste en crear una clase auxiliar en la capa DAL que contiene el `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` método. Este método crearía una instancia de la `CategoriesTableAdapter` y `ProductsTableAdapter` y, a continuación, establezca estos dos TableAdapters `Connection` propiedades al mismo `SqlConnection` instancia. En ese punto, uno de los dos TableAdapters iniciaba la transacción con una llamada a `BeginTransaction`. Los métodos de TableAdapter para reasignar los productos y eliminar la categoría se invocaría en un `try...catch` bloque con la transacción confirma o revierte hacia atrás, según sea necesario.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Paso 4: Agregar el`UpdateWithTransaction`método a la capa de lógica de negocios

En el paso 3 agregamos una `UpdateWithTransaction` método a la `ProductsTableAdapter` en la capa DAL. Se debe agregar un método correspondiente a la capa BLL. Mientras que el nivel de presentación podría llamar directamente a la capa DAL para invocar la `UpdateWithTransaction` método, estos tutoriales han emprendido definir una arquitectura por niveles que aísla la capa DAL desde la capa de presentación. Por consiguiente, responsabilidad nos seguir este enfoque.

Abra la `ProductsBLL` archivo de clase y agregue un método denominado `UpdateWithTransaction` que llama simplemente hasta el método correspondiente de la capa DAL. Ahora debería haber dos nuevos métodos de `ProductsBLL`: `UpdateWithTransaction`, que acaba de agregar, y `DeleteProductsWithTransaction`, que se agregó en el paso 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Estos métodos no incluyen la `DataObjectMethodAttribute` atributo asignado a la mayoría de los otra métodos en el `ProductsBLL` clase porque se va a invocar estos métodos directamente desde las clases de código subyacente de páginas ASP.NET. Recuerde que `DataObjectMethodAttribute` se usa para marcar los métodos deben aparecer en las operaciones de asignación ObjectDataSource Configurar origen de datos asistente y en la pestaña (SELECT, UPDATE, INSERT o DELETE). Puesto que el control GridView no tiene ninguna compatibilidad integrada para lote editen o eliminen, tenemos invocar estos métodos mediante programación, en lugar de usar el enfoque sin código declarativo.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Paso 5: Atómicamente actualizar bases de datos de la capa de presentación

Para ilustrar el efecto de la transacción al actualizar un lote de registros, s permiten crear una interfaz de usuario que muestra todos los productos en un GridView e incluye un botón Web controlar que, al hacer clic, los productos se reasignan `CategoryID` valores. En concreto, se centrará la reasignación de categoría para que los primeros productos varios se asignan válido `CategoryID` valor, mientras que otras son muy asigna un inexistente `CategoryID` valor. Si se intenta actualizar la base de datos con un producto cuyo `CategoryID` no coincide con una categoría existente s `CategoryID`, se producirá una infracción de restricción de clave externa y se producirá una excepción. Lo veremos en este ejemplo es que, cuando utiliza una transacción de la excepción que se generan a partir de la infracción de restricción de clave externa provocará el anterior válida `CategoryID` cambios se revierta. Si no usa una transacción, sin embargo, las modificaciones a las categorías iniciales permanecerá.

Comience abriendo la `Transactions.aspx` página en el `BatchData` carpeta y arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Establecer su `ID` a `Products` y, en su etiqueta inteligente, enlazarla a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para extraer los datos de la `ProductsBLL` clase s. `GetProducts` método. Esto eliminará ser un GridView de solo lectura, por lo que establece las listas de lista desplegable en la actualización, INSERCIÓN y las fichas (ninguno) y haga clic en Finalizar.


[![Figura 5: Configurar el ObjectDataSource para utilizar el método de la clase ProductsBLL s GetProducts](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: figura 5: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase s `GetProducts` método ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las pestañas en (ninguno)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: establecer las listas de lista desplegable en la actualización, INSERCIÓN y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y un CampoCasillaVerificación para los campos de datos de producto. Quitar todos estos campos excepto `ProductID`, `ProductName`, `CategoryID`, y `CategoryName` y cambiar el nombre de la `ProductName` y `CategoryName` BoundFields `HeaderText` propiedades para el producto y categoría, respectivamente. Desde la etiqueta inteligente, active la opción Habilitar paginación. Después de realizar estas modificaciones, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

A continuación, agregue tres controles de botón Web situados encima de GridView. Establecer el primer botón s Text (propiedad) para actualizar la cuadrícula, la segunda s para modificar categorías (con la transacción) y la una tercera s para modificar categorías (sin transacción).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

En este momento la vista de diseño en Visual Studio debe ser similar a la pantalla se muestra en la figura 7.


[![La página contiene una GridView y tres controles de botón Web](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: la página contiene una GridView y tres controles Button de Web ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Crear controladores de eventos para cada uno de los tres botón s `Click` eventos y utilice el código siguiente:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

La actualización de botón s `Click` controlador de eventos simplemente vuelve a enlazar los datos en GridView mediante una llamada a la `Products` GridView s `DataBind` método.

El segundo controlador de eventos reasigna los productos `CategoryID` s y se utiliza el nuevo método de transacción de la capa BLL para realizar la base de datos actualiza bajo el paraguas de una transacción. Tenga en cuenta que cada producto s `CategoryID` arbitrariamente se establece en el mismo valor que su `ProductID`. Esto funcionará sin problemas para el primer algunos productos, ya que dichos productos tienen `ProductID` valores que se producen asignar a válido `CategoryID` s. Una vez, pero la `ProductID` inicio s haga demasiado grande, esta superposición una coincidencia de `ProductID` s y `CategoryID` s ya no es aplicable.

La tercera `Click` controlador de eventos actualiza los productos `CategoryID` s de la misma manera, pero envía la actualización a la base de datos mediante la `ProductsTableAdapter` predeterminado s. `Update` método. Esto  `Update` /método siguiente no se ajusta a la serie de comandos dentro de una transacción, por lo que estos cambios se realizan antes el primer error de infracción de restricción de clave externa se encontró se conservarán.

Para mostrar este comportamiento, visite esta página a través de un explorador. Inicialmente debería ver la primera página de datos tal como se muestra en la figura 8. A continuación, haga clic en el botón Modificar categorías (con la transacción). Esto provocará una devolución de datos y al intentar actualizar todos los productos `CategoryID` valores, pero se producirá una infracción de restricción de clave externa (consulte la figura 9).


[![Los productos se muestran en un control GridView paginable](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: los productos se muestran en un control GridView paginable ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Reasignar los resultados de categorías en una infracción de restricción de clave externa](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: reasignar los resultados de categorías en una infracción de restricción de clave externa ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


A continuación, pulse el botón Atrás del explorador s y, a continuación, haga clic en el botón Actualizar la cuadrícula. Tras la actualización de los datos debería ver el mismo resultado exacto tal como se muestra en la figura 8. Es decir, incluso aunque algunos de los productos `CategoryID` s se valores cambiados para legal y se actualiza en la base de datos, se revierten cuando se produjo la infracción de restricción de clave externa.

Ahora, intente hacer clic en el botón Modificar categorías (sin transacción). El resultado será el mismo error de infracción de restricción de clave externa (consulte la figura 9), pero esta vez los productos cuyo `CategoryID` cambiaron los valores para legal de un valor no se revertirá. Presione el botón Atrás del explorador s y, a continuación, en el botón de actualización de cuadrícula. Como se muestra en la figura 10, la `CategoryID` ha se ha reasignado s de los ocho primeros productos. Por ejemplo, en la figura 8, cambios tenían un `CategoryID` de 1, pero en la figura 10 TI s se ha reasignado a 2.


[![Algunos valores de Id. de categoría de productos actualizado mientras otros usuarios se han no estaban](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: algunos productos `CategoryID` valores actualizan mientras otros usuarios se han no estaban ([haga clic aquí para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Resumen

De forma predeterminada, los métodos de TableAdapter s no incluya las instrucciones ejecutadas de la base de datos dentro del ámbito de una transacción, pero con un poco de trabajo podemos agregar métodos que va a crear, commit y rollback una transacción. En este tutorial, creamos tres tales métodos en la `ProductsTableAdapter` clase: `BeginTransaction`, `CommitTransaction`, y `RollbackTransaction`. Hemos visto cómo usar estos métodos junto con un `try...catch` bloque para realizar una serie de instrucciones de modificación de datos atómica. En concreto, creamos la `UpdateWithTransaction` método en el `ProductsTableAdapter`, que usa el patrón de actualización por lotes para realizar las modificaciones necesarias en las filas de un proporcionado `ProductsDataTable`. También hemos agregado la `DeleteProductsWithTransaction` método a la `ProductsBLL` clase en el BLL, que acepta un `List` de `ProductID` valores como entrada y llama al método de patrón de DB-Direct `Delete` para cada `ProductID`. Ambos métodos empiece por crear una transacción y, a continuación, ejecutar las instrucciones de modificación de datos dentro de un `try...catch` bloque. Si se produce una excepción, se revierte la transacción, en caso contrario, se confirma.

Paso 5 muestra el efecto de las actualizaciones de lote transaccional frente a las actualizaciones por lotes que dejó de usar una transacción. En los siguientes tres tutoriales se se se basan en la base que se disponen en este tutorial y crear interfaces de usuario para realizar inserciones, eliminaciones y actualizaciones por lotes.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Mantiene la coherencia de la base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Procedimientos almacenados de administración de transacciones en SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transacciones realizadas fácil:`System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope y DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Usar transacciones de bases de datos de Oracle en .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Dave Gardner, Hilton Giesenow y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Siguiente](batch-updating-cs.md)
