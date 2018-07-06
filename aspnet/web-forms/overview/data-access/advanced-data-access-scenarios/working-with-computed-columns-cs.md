---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Trabajar con columnas calculadas (C#) | Microsoft Docs
author: rick-anderson
description: Al crear una tabla de base de datos, Microsoft SQL Server le permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente referencia...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: cde6b829bb8a4ac4a07ff5b5e519444a4cd99a01
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816672"
---
<a name="working-with-computed-columns-c"></a>Trabajar con columnas calculadas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) o [descargar PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Al crear una tabla de base de datos, Microsoft SQL Server permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente hace referencia a otros valores en el mismo registro de base de datos. Estos valores son de solo lectura en la base de datos, lo que requiere consideraciones especiales cuando se trabaja con los TableAdapters. En este tutorial se aprenderá cómo afrontar los desafíos que suponen las columnas calculadas.


## <a name="introduction"></a>Introducción

Microsoft SQL Server permite  *[columnas calculadas](https://msdn.microsoft.com/library/ms191250.aspx)*, que son columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas de la misma tabla. Por ejemplo, un modelo de datos de seguimiento de tiempo podría tener una tabla denominada `ServiceLog` con columnas incluidas `ServicePerformed`, `EmployeeID`, `Rate`, y `Duration`, entre otros. Mientras que la cantidad de vencimiento por servicio (que se va a la tasa de multiplicado por la duración) podría calcularse a través de una página web o de otra interfaz programática, podría ser útil para incluir una columna en la `ServiceLog` tabla denominada `AmountDue` que notifica esto información. Esta columna podría crearse como una columna normal, pero deberá actualizarse en cualquier momento el `Rate` o `Duration` les cambiados los valores de columna. Un mejor enfoque sería realizar la `AmountDue` una columna calculada mediante la expresión de columna `Rate * Duration`. Hacerlo podría causar SQL Server calcular automáticamente el `AmountDue` valor de la columna cada vez que se hizo referencia a una consulta.

Puesto que un valor de columna calculada s viene determinada por una expresión, estas columnas son de solo lectura y, por tanto, no pueden tener los valores asignados a ellos en `INSERT` o `UPDATE` instrucciones. Sin embargo, cuando las columnas calculadas forman parte de la consulta principal de un TableAdapter utiliza instrucciones SQL ad hoc, se incluyen automáticamente en el autogenerado `INSERT` y `UPDATE` instrucciones. Por lo tanto, los TableAdapter s `INSERT` y `UPDATE` consultas y `InsertCommand` y `UpdateCommand` propiedades deben actualizarse para quitar las referencias a las columnas calculadas.

Uno de los retos del uso de calcula las columnas con un TableAdapter utiliza instrucciones SQL ad hoc es que los TableAdapter s `INSERT` y `UPDATE` automáticamente las consultas se vuelven a generar siempre que se complete el Asistente para configuración de TableAdapter. Por lo tanto, las columnas calculadas quitan manualmente desde el `INSERT` y `UPDATE` consultas volverá a aparecer si se vuelve a ejecutar el asistente. Aunque los TableAdapters que utilizan procedimientos almacenados don t sufren este fragilidad, tienen sus propias peculiaridades que abordaremos en el paso 3.

En este tutorial se agregará una columna calculada a la `Suppliers` de tabla en la base de datos Northwind y, a continuación, creará un TableAdapter correspondiente para que funcione con esta tabla y su columna calculada. Tendremos nuestro TableAdapter utilizar procedimientos almacenados en lugar de instrucciones SQL ad hoc para que nuestro t de las personalizaciones se pierde cuando se usa el Asistente para configuración de TableAdapter.

Introducción s Let!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Paso 1: Agregar una columna calculada a la`Suppliers`tabla

La base de datos Northwind no tiene ninguna columna calculada por lo que necesitamos agregar una nosotros mismos. Para este tutorial permite s Agregar una columna calculada a la `Suppliers` tabla denominada `FullContactName` que devuelve el nombre de contacto s, título y la compañía que trabajan para el siguiente formato: `ContactName` (`ContactTitle`, `CompanyName`). Esto calcula la columna podría utilizarse en los informes al mostrar información sobre los proveedores.

Comience abriendo la `Suppliers` definición de tabla con el botón secundario en el `Suppliers` de tabla en el Explorador de servidores y elija Abrir definición de tabla en el menú contextual. Esto mostrará las columnas de la tabla y sus propiedades, como su tipo de datos, si permiten `NULL` s y así sucesivamente. Para agregar una columna calculada, empiece escribiendo el nombre de la columna en la definición de tabla. A continuación, escriba la expresión en el cuadro de texto (fórmula) en la sección de la especificación de columna calculada en la ventana Propiedades de columna (consulte la figura 1). Nombre de la columna calculada `FullContactName` y utilice la siguiente expresión:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Tenga en cuenta que se pueden concatenar cadenas en SQL mediante el `+` operador. El `CASE` instrucción se puede usar como una instrucción condicional en un lenguaje de programación tradicional. En la expresión anterior el `CASE` instrucción se puede leer como: si `ContactTitle` no `NULL` de salida, a continuación, el `ContactTitle` valor concatenado con una coma, en caso contrario, emitir nada. Para obtener más información sobre la utilidad de la `CASE` instrucción, consulte [la potencia de SQL `CASE` instrucciones](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> En lugar de usar un `CASE` instrucción aquí, podríamos haber también usado `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Devuelve *checkExpression* si es distinto de NULL, de lo contrario devuelve *replacementValue*. Aunque cualquiera `ISNULL` o `CASE` funcionará en este caso, existen escenarios más complejos donde la flexibilidad de la `CASE` instrucción no puede coincidir con `ISNULL`.


Después de agregar esta columna calculada, la pantalla debería parecerse a la pantalla en la figura 1.


[![Agregar una columna calculada denominada FullContactName a la tabla Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: agregar una columna calculada denominada `FullContactName` a la `Suppliers` tabla ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image3.png))


Después de la columna calculada de nomenclatura y especificar su expresión, guardar los cambios en la tabla haciendo clic en el icono Guardar en la barra de herramientas, presionando Ctrl + S o, en el menú archivo y elija Guardar `Suppliers`.

Guardar la tabla debe actualizar el Explorador de servidores, incluida la columna recién agregados en el `Suppliers` lista de columnas de la tabla s. Además, se ajustará automáticamente la expresión escrita en el cuadro de texto (fórmula) en una expresión equivalente que elimina espacios en blanco innecesarios, rodea los nombres de columna entre corchetes (`[]`) e incluye los paréntesis para mostrar más explícitamente el orden de operaciones:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Para obtener más información sobre las columnas calculadas de Microsoft SQL Server, consulte el [documentación técnica](https://msdn.microsoft.com/library/ms191250.aspx). Consulte también el [Cómo: especificar columnas calculadas](https://msdn.microsoft.com/library/ms188300.aspx) para ver un tutorial paso a paso de creación de columnas calculadas.

> [!NOTE]
> De forma predeterminada, las columnas calculadas no se almacenan físicamente en la tabla, pero en su lugar, se vuelven a calcular cada vez que se hace referencia a una consulta. Activando la casilla de verificación se conserva, sin embargo, puede indicar a SQL Server para almacenar físicamente la columna calculada en la tabla. Esto permite que un índice que se crearán en la columna calculada, lo que puede mejorar el rendimiento de las consultas que utilizan el valor de columna calculada en sus `WHERE` cláusulas. Consulte [crear índices en columnas calculadas](https://msdn.microsoft.com/library/ms189292.aspx) para obtener más información.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Paso 2: Ver los valores de columna calculada s

Antes de que se inicia el trabajo en la capa de acceso a datos, permiten s Tómese un minuto para ver el `FullContactName` valores. Desde el Explorador de servidores, haga doble clic en el `Suppliers` nombre de la tabla y elija la nueva consulta en el menú contextual. Se abrirá una ventana de consulta que nos pedirá que elija qué tablas se deben incluir en la consulta. Agregue el `Suppliers` de tabla y haga clic en Cerrar. A continuación, compruebe el `CompanyName`, `ContactName`, `ContactTitle`, y `FullContactName` las columnas de la tabla Suppliers. Por último, haga clic en el icono de exclamación rojo en la barra de herramientas para ejecutar la consulta y ver los resultados.

Como se muestra en la figura 2, los resultados incluyen `FullContactName`, que muestra la `CompanyName`, `ContactName`, y `ContactTitle` columnas mediante el formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![El FullContactName utiliza formato ContactName (título de contacto, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: el `FullContactName` utiliza el formato `ContactName` (`ContactTitle`, `CompanyName`) ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Paso 3: Agregar el`SuppliersTableAdapter`a la capa de acceso a datos

Para trabajar con la información del proveedor en nuestra aplicación es necesario crear primero un TableAdapter y DataTable en nuestro DAL. Idealmente, esto se logra utilizando los mismos pasos sencillo examinados en los tutoriales anteriores. Sin embargo, trabajar con columnas calculadas presenta unas arrugas que merecen explicación.

Si utiliza un TableAdapter utiliza instrucciones SQL ad hoc, simplemente puede incluir la columna calculada en la consulta principal de TableAdapter s mediante el Asistente para configuración de TableAdapter. Esto, sin embargo, generará automáticamente `INSERT` y `UPDATE` las instrucciones que incluyan la columna calculada. Si intenta ejecutar uno de estos métodos, un `SqlException` con el mensaje de la columna *ColumnName* no se puede modificar porque es una columna calculada o se producirá el resultado de un operador UNION. Mientras el `INSERT` y `UPDATE` instrucción se puede ajustar manualmente a través de los TableAdapters `InsertCommand` y `UpdateCommand` propiedades, estas personalizaciones se perderán cuando se vuelve a ejecutar el Asistente para configuración de TableAdapter.

Debido a la fragilidad de los TableAdapter utiliza instrucciones SQL ad hoc, se recomienda que usamos los procedimientos almacenados cuando se trabaja con columnas calculadas. Si utiliza procedimientos almacenados existentes, simplemente configure TableAdapter como se describe en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial. Sin embargo, si tiene el Asistente de TableAdapter para crear los procedimientos almacenados para usted, es importante inicialmente omitir las columnas calculadas de la consulta principal. Si incluye una columna calculada en la consulta principal, el Asistente para configuración de TableAdapter le informará, tras la finalización, que no puede crear los procedimientos almacenados correspondientes. En resumen, es necesario configurar inicialmente el TableAdapter utilizando una consulta principal libre de columnas calculada y, a continuación, actualice manualmente el procedimiento almacenado correspondiente y los TableAdapters `SelectCommand` para incluir la columna calculada. Este enfoque es similar al utilizado en el [actualizar TableAdapter para usar](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* tutorial.

Para este tutorial, permiten s Agregar un nuevo TableAdapter y hacer que se cree automáticamente los procedimientos almacenados para nosotros. Por lo tanto, tendremos que inicialmente se omite la `FullContactName` columna calculada de la consulta principal.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos en el `~/App_Code/DAL` carpeta. Haga clic en el diseñador y, en el menú contextual, elija Agregar un nuevo TableAdapter. Esto iniciará al Asistente para configuración de TableAdapter. Especificar la base de datos para consultar datos desde (`NORTHWNDConnectionString` desde `Web.config`) y haga clic en siguiente. Dado que no hemos creado todavía ningún procedimiento almacenado para consultar o modificar el `Suppliers` tabla, seleccione el crear nuevos procedimientos almacenados de opción para que el asistente crea para nosotros y haga clic en siguiente.


[![Elija los procedimientos almacenados nueva opción de crear](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: elija los procedimientos almacenados nueva opción de crear ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image9.png))


El paso posterior nos pide la consulta principal. Escriba la siguiente consulta, que devuelve el `SupplierID`, `CompanyName`, `ContactName`, y `ContactTitle` columnas para cada proveedor. Tenga en cuenta que esta consulta intencionadamente omite la columna calculada (`FullContactName`); se actualizará el procedimiento almacenado correspondiente para incluir esta columna en el paso 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Después de escribir la consulta principal y hacer clic en siguiente, el Asistente nos permite nombrar los cuatro procedimientos almacenados que generará. Nombre de estos procedimientos almacenados `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, y `Suppliers_Delete`, como se muestra en la figura 4.


[![Personalizar los nombres de los procedimientos almacenados generados automáticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizar los nombres de los procedimientos almacenados de Auto-Generated ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image12.png))


El siguiente paso del asistente nos permite a los métodos de TableAdapter s el nombre y especifique los patrones que se usa para obtener acceso y actualizar los datos. Deje todas las tres casillas de verificación activadas, pero cambie el nombre de la `GetData` método `GetSuppliers`. Haga clic en Finalizar para completar al asistente.


[![Cambie el nombre del método GetData a GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: cambiar el nombre de la `GetData` método `GetSuppliers` ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image15.png))


Al hacer clic en Finalizar, el asistente creará los cuatro procedimientos almacenados y agregar el TableAdapter y la correspondiente DataTable al conjunto de datos con tipo.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Paso 4: Incluida la columna calculada en la consulta principal de TableAdapter s

Ahora necesitamos actualizar TableAdapter y DataTable que creó en el paso 3 para incluir la `FullContactName` columna calculada. Esto implica dos pasos:

1. Actualizando el `Suppliers_Select` procedimiento almacenado para devolver el `FullContactName` columna calculada, y
2. Actualizar para incluir la correspondiente DataTable `FullContactName` columna.

Comience por navegar en el Explorador de servidores y explore en profundidad la carpeta procedimientos almacenados. Abra el `Suppliers_Select` procedimiento almacenado y actualización de la `SELECT` consulta incluya el `FullContactName` columna calculada:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Guardar los cambios en el procedimiento almacenado, haga clic en el icono Guardar en la barra de herramientas, presionando Ctrl + S o eligiendo el guardar `Suppliers_Select` opción en el menú archivo.

A continuación, vuelva al diseñador de DataSet, haga doble clic en el `SuppliersTableAdapter`y elija Configurar en el menú contextual. Tenga en cuenta que el `Suppliers_Select` columna ahora incluye la `FullContactName` columna en su colección de columnas de datos.


[![Ejecute el Asistente para configuración de TableAdapter s para actualizar las columnas de DataTable s](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: ejecutar el Asistente para configuración para actualizar las columnas de DataTable s s de TableAdapter ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image18.png))


Haga clic en Finalizar para completar al asistente. Esto agregará automáticamente una columna correspondiente a la `SuppliersDataTable`. El Asistente de TableAdapter es lo suficientemente inteligente como para detectar que la `FullContactName` columna es una columna calculada y, por tanto, de solo lectura. Por lo tanto, Establece la columna s `ReadOnly` propiedad `true`. Para comprobarlo, seleccione la columna de la `SuppliersDataTable` y, a continuación, vaya a la ventana Propiedades (consulte la figura 7). Tenga en cuenta que el `FullContactName` columna s `DataType` y `MaxLength` propiedades también se establecen en consecuencia.


[![La columna FullContactName está marcada como de solo lectura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: el `FullContactName` columna está marcada como de solo lectura ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Paso 5: Agregar un`GetSupplierBySupplierID`método a TableAdapter

En este tutorial crearemos una página ASP.NET que muestra los proveedores en una cuadrícula actualizable. En más allá de los tutoriales hemos actualizado un único registro de la capa de lógica empresarial mediante la recuperación que realizar una copia en la capa DAL para propagar los cambios en registro concreto de la capa DAL como DataTable fuertemente tipado, actualizar sus propiedades y, a continuación, enviar la DataTable actualizada la base de datos. Para lograr este primer paso - recuperar del registro que se va a actualizar desde la capa DAL: tenemos que agregar primero un `GetSupplierBySupplierID(supplierID)` método a la capa DAL.

Haga doble clic en el `SuppliersTableAdapter` en el diseño de conjunto de datos y elija la opción de Agregar consulta en el menú contextual. Como hicimos en el paso 3, permiten el Asistente para generar un nuevo procedimiento almacenado para que podamos seleccionando la opción de crear nuevo procedimiento almacenado (consulte la vuelta a la figura 3 para captura de pantalla de este paso del asistente). Puesto que este método devolverá un registro con varias columnas, indican que desea usar una consulta SQL que es una instrucción SELECT que devuelve filas y haga clic en siguiente.


[![Elija la selección que devuelve filas, opción](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: elija la selección que devuelve filas opción ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image24.png))


El paso posterior nos pedirá para la consulta que se utilizará para este método. Escriba lo siguiente, que devuelve los mismos campos de datos como la consulta principal, pero de un determinado proveedor.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

La siguiente pantalla nos pide el nombre del procedimiento almacenado que se generarán automáticamente. Nombre de este procedimiento almacenado `Suppliers_SelectBySupplierID` y haga clic en siguiente.


[![Nombre del procedimiento almacenado de Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nombre del procedimiento almacenado `Suppliers_SelectBySupplierID` ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image27.png))


Por último, las instrucciones del asistente nos para los datos de acceso a patrones y los nombres de método que se usará para el TableAdapter. Deje ambas casillas seleccionadas, pero cambie el nombre de la `FillBy` y `GetDataBy` métodos para `FillBySupplierID` y `GetSupplierBySupplierID`, respectivamente.


[![Nombre de la FillBySupplierID de métodos de TableAdapter y GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: el nombre de los métodos de TableAdapter `FillBySupplierID` y `GetSupplierBySupplierID` ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image30.png))


Haga clic en Finalizar para completar al asistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Paso 6: Creación de la capa de lógica de negocios

Antes de crear una página ASP.NET que usa la columna calculada que creó en el paso 1, primero es necesario agregar los métodos correspondientes en la capa BLL. Nuestra página ASP.NET, que creamos en el paso 7, permitirá a los usuarios ver y editar los proveedores. Por lo tanto, tenemos nuestro BLL proporcionar, como mínimo, un método para obtener todos los proveedores y otro para actualizar un proveedor determinado.

Crear un nuevo archivo de clase denominado `SuppliersBLLWithSprocs` en el `~/App_Code/BLL` carpeta y agregue el código siguiente:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Al igual que las clases BLL, `SuppliersBLLWithSprocs` tiene un `protected` `Adapter` propiedad que devuelve una instancia de la `SuppliersTableAdapter` clase junto con dos `public` métodos: `GetSuppliers` y `UpdateSupplier`. El `GetSuppliers` método llama y devuelve el `SuppliersDataTable` devuelto por la correspondiente `GetSupplier` método en la capa de acceso a datos. El `UpdateSupplier` método recupera información sobre el proveedor particular está actualizando mediante una llamada a la s DAL `GetSupplierBySupplierID(supplierID)` método. A continuación, actualiza el `CategoryName`, `ContactName`, y `ContactTitle` propiedades y confirma estos cambios en la base de datos mediante una llamada a la capa de acceso a datos de s `Update` método, pasando el modificado `SuppliersRow` objeto.

> [!NOTE]
> Excepto para `SupplierID` y `CompanyName`, permitir que todas las columnas de la tabla Suppliers `NULL` valores. Por lo tanto, si en el pasado `contactName` o `contactTitle` parámetros son `null` es necesario establecer la correspondiente `ContactName` y `ContactTitle` propiedades para un `NULL` valor de base de datos mediante el `SetContactNameNull` y `SetContactTitleNull`métodos, respectivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Paso 7: Trabajar con la columna calculada de la capa de presentación

Con la columna calculada que se agrega a la `Suppliers` tabla y DAL y BLL se actualizan en consecuencia, estamos preparados crear una página ASP.NET que funciona con la `FullContactName` columna calculada. Comience abriendo la `ComputedColumns.aspx` página en el `AdvancedDAL` carpetas y arrastre un control GridView del cuadro de herramientas hasta el diseñador. Establezca la s GridView `ID` propiedad `Suppliers` y, en la etiqueta inteligente, de enlazarla a un nuevo origen ObjectDataSource denominado `SuppliersDataSource`. Configurar el origen ObjectDataSource para usar el `SuppliersBLLWithSprocs` clase agregamos copia en el paso 6 y haga clic en siguiente.


[![Configurar el origen ObjectDataSource para usar la clase SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurar el origen ObjectDataSource que se usarán el `SuppliersBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image33.png))


Hay solo dos métodos definidos en el `SuppliersBLLWithSprocs` clase: `GetSuppliers` y `UpdateSupplier`. Asegúrese de que estos dos métodos se especifican en la instrucción SELECT y UPDATE fichas, respectivamente y haga clic en Finalizar para completar la configuración del origen ObjectDataSource.

Tras la finalización del Asistente para configuración de orígenes de datos, Visual Studio agregará BoundField para cada uno de los campos de datos devueltos. Quitar el `SupplierID` BoundField y cambie el `HeaderText` propiedades de la `CompanyName`, `ContactName`, `ContactTitle`, y `FullContactName` BoundFields para la empresa, póngase en contacto con nombre, título y póngase en contacto con el nombre completo, respectivamente. En la etiqueta inteligente, active la casilla Habilitar edición para activar las capacidades de edición de GridView s integradas.

Además de agregar BoundFields a GridView, finalizar el Asistente de origen de datos también hace que Visual Studio establecer la s ObjectDataSource `OldValuesParameterFormatString` propiedad original\_{0}. Revertir esta configuración en su valor predeterminado, {0} .

Después de realizar estas modificaciones en el control GridView y ObjectDataSource, su marcado declarativo debe tener un aspecto similar al siguiente:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

A continuación, visite esta página a través de un explorador. Como se muestra en la figura 12, cada proveedor se muestra en una cuadrícula que incluye el `FullContactName` formato de columna, cuyo valor es simplemente la concatenación de las tres columnas `ContactName` (`ContactTitle`, `CompanyName`).


[![Cada proveedor se muestra en la cuadrícula](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: cada proveedor se muestra en la cuadrícula ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image36.png))


Al hacer clic en el botón Editar para un determinado proveedor produce un postback y presenta esa fila en su edición de la interfaz (consulte la figura 13). Las tres primeras columnas se representan en su interfaz de edición de predeterminado: control de un cuadro de texto cuya `Text` propiedad está establecida en el valor del campo de datos. La `FullContactName` columna, sin embargo, permanece como texto. Cuando el BoundFields se agregaron en el control GridView en la finalización del Asistente para configuración de orígenes de datos, el `FullContactName` BoundField s `ReadOnly` propiedad se estableció en `true` porque correspondiente `FullContactName` columna en el `SuppliersDataTable` tiene su `ReadOnly` propiedad establecida en `true`. Como se indicó en el paso 4, el `FullContactName` s `ReadOnly` propiedad se estableció en `true` porque el TableAdapter detectó que la columna es una columna calculada.


[![La columna FullContactName no es modificable](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: el `FullContactName` columna no es modificable ([haga clic aquí para ver imagen en tamaño completo](working-with-computed-columns-cs/_static/image39.png))


Siga adelante y actualizar el valor de una o varias de las columnas que se puede editables y haga clic en actualizar. Tenga en cuenta cómo el `FullContactName` valor s se actualiza automáticamente para reflejar el cambio.

> [!NOTE]
> GridView actualmente usa BoundFields para los campos editables, lo que en la interfaz de edición de forma predeterminada. Puesto que el `CompanyName` campo es obligatorio, se debe convertir en TemplateField que incluye un control RequiredFieldValidator. Dejar como un ejercicio para el lector interesado. Consulte la [agregar controles de validación a las Interfaces de inserción y edición](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para obtener instrucciones paso a paso sobre convertir BoundField en TemplateField y agregar controles de validación.


## <a name="summary"></a>Resumen

Al definir el esquema para una tabla, Microsoft SQL Server permite la inclusión de columnas calculadas. Estas son columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas en el mismo registro. Desde los valores para las columnas calculadas se basan en una expresión, que son de solo lectura y no se puede asignar un valor en un `INSERT` o `UPDATE` instrucción. Esto presenta desafíos cuando se usa una columna calculada en la consulta principal de un TableAdapter que intenta generar automáticamente correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones.

En este tutorial se describen técnicas para sortear los desafíos que suponen las columnas calculadas. En concreto, hemos usado los procedimientos almacenados en nuestra TableAdapter para superar la fragilidad inherente en los TableAdapter utiliza instrucciones SQL ad hoc. Al tener el Asistente de TableAdapter para crear nuevos procedimientos almacenados, es importante que tenemos la consulta principal inicialmente omitir ninguna columna calculada porque su presencia impide que los procedimientos almacenados de modificación de datos que se está generando. Después de que el TableAdapter se ha configurado inicialmente, su `SelectCommand` procedimiento almacenado puede ser reformó para incluir las columnas calculadas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Hilton Geisenow y Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-additional-datatable-columns-cs.md)
> [Siguiente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
