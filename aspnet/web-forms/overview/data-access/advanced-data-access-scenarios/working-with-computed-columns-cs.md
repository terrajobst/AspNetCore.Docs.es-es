---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Trabajar con columnas calculadas (C#) | Documentos de Microsoft
author: rick-anderson
description: Al crear una tabla de base de datos, Microsoft SQL Server le permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente referencia...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a67abd2a0c140c0503c07f764549a6d90ef7298
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="working-with-computed-columns-c"></a>Trabajar con columnas calculadas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) o [descarga de PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Al crear una tabla de base de datos, Microsoft SQL Server permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente hace referencia a otros valores en el mismo registro de base de datos. Estos valores son de solo lectura en la base de datos, lo que requiere consideraciones especiales al trabajar con TableAdapters. En este tutorial se muestra cómo cumplir los desafíos que suponen las columnas calculadas.


## <a name="introduction"></a>Introducción

Microsoft SQL Server permite  *[las columnas calculadas](https://msdn.microsoft.com/library/ms191250.aspx)*, que son columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas de la misma tabla. Por ejemplo, una vez el modelo de datos de seguimiento podría tener una tabla denominada `ServiceLog` con columnas incluidas `ServicePerformed`, `EmployeeID`, `Rate`, y `Duration`, entre otros. Mientras el importe a pagar por cada servicio (que se va a la velocidad multiplicada por la duración) se ha podido calcular a través de una página web u otra interfaz de programación, puede resultar útil para incluir una columna en el `ServiceLog` tabla denominada `AmountDue` que notifica este información. Esta columna se pudo crear una columna normal, pero solo debería actualizarse en cualquier momento la `Rate` o `Duration` cambiados los valores de columna. Un enfoque más adecuado sería asegurarse el `AmountDue` una columna calculada usando la expresión de columna `Rate * Duration`. Si lo hace, haría que SQL Server para que calcule automáticamente el `AmountDue` valor de la columna cada vez que se hizo referencia a una consulta.

Puesto que un valor de columna calculada s está determinado por una expresión, estas columnas son de solo lectura y, por tanto, no pueden tener valores asignados a ellos en `INSERT` o `UPDATE` las instrucciones. Sin embargo, cuando las columnas calculadas forman parte de la consulta principal de un TableAdapter que utiliza instrucciones SQL ad hoc, se incluyen automáticamente en el generado automáticamente `INSERT` y `UPDATE` las instrucciones. Por lo tanto, los TableAdapter s `INSERT` y `UPDATE` consultas y `InsertCommand` y `UpdateCommand` propiedades deben actualizarse para quitar las referencias a las columnas calculadas.

Presenta la dificultad de usar las columnas con un TableAdapter que utiliza instrucciones SQL ad hoc calculadas es que los TableAdapter s `INSERT` y `UPDATE` consultas se vuelven a generar automáticamente siempre que se complete el Asistente para configuración de TableAdapter. Por lo tanto, las columnas calculadas quitan manualmente desde el `INSERT` y `UPDATE` consultas volverá a aparecer si se vuelve a ejecutar el asistente. Aunque los TableAdapters que utilizan procedimientos almacenados don t sufren este fragilidad, tienen sus propias peculiaridades que se solucionará en el paso 3.

En este tutorial se agregará una columna calculada a la `Suppliers` de la tabla en la base de datos Northwind y, a continuación, crear un TableAdapter correspondiente para trabajar con esta tabla y la columna calculada. Tenemos nuestro TableAdapter usar procedimientos almacenados en lugar de instrucciones SQL ad hoc para que nuestro t de las personalizaciones se pierde cuando se utiliza el Asistente para configuración de TableAdapter.

Permiten s empiece a trabajar.

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Paso 1: Agregar una columna calculada a la`Suppliers`tabla

La base de datos Northwind no tiene las columnas calculadas por lo que necesitamos agregar una nosotros mismos. Para este tutorial permite s Agregar una columna calculada a la `Suppliers` tabla denominada `FullContactName` que devuelve el nombre de contacto s, título y la empresa que trabajan en el siguiente formato: `ContactName` (`ContactTitle`, `CompanyName`). Esto calcula la columna podría utilizarse en los informes para mostrar información sobre los proveedores.

Comience abriendo la `Suppliers` definición de tabla con el botón secundario en el `Suppliers` de tabla en el Explorador de servidores y elija Abrir definición de tabla en el menú contextual. Esto mostrará las columnas de la tabla y sus propiedades, como su tipo de datos, si se permiten `NULL` s y así sucesivamente. Para agregar una columna calculada, empiece escribiendo el nombre de la columna en la definición de tabla. A continuación, escriba la expresión en el cuadro de texto (fórmula) en la sección de la especificación de columna calculada en la ventana Propiedades de columna (consulte la figura 1). Nombre de la columna calculada `FullContactName` y utilice la siguiente expresión:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Tenga en cuenta que se pueden concatenar cadenas en SQL mediante el `+` operador. El `CASE` instrucción puede utilizarse como una instrucción condicional en un lenguaje de programación tradicional. En la expresión anterior la `CASE` instrucción puede leerse como: si `ContactTitle` no `NULL` de salida, a continuación, el `ContactTitle` valor concatenado con una coma, en caso contrario emitir nada. Para obtener más información sobre la utilidad de la `CASE` instrucción, consulte [Power de SQL `CASE` instrucciones](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> En lugar de usar un `CASE` instrucción aquí, podríamos haber también utilizado `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Devuelve *checkExpression* si es distinto de NULL, de lo contrario devuelve *Valordereemplazo*. Mientras una `ISNULL` o `CASE` funcionará en este caso, hay más intrincados escenarios donde la flexibilidad de la `CASE` instrucción no puede coincidir con `ISNULL`.


Después de agregar esta columna calculada, la pantalla debe ser similar a la pantalla que se captura en la figura 1.


[![Agregar una columna calculada denominada FullContactName a la tabla de proveedores](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: agregar una columna calculada denominada `FullContactName` a la `Suppliers` tabla ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image3.png))


Después de la columna calculada de nomenclatura y especificar la expresión, guardar los cambios a la tabla haciendo clic en el icono Guardar en la barra de herramientas, presionando Ctrl + S o desde el menú archivo y elegir guardar `Suppliers`.

Al guardar la tabla debe actualizar el Explorador de servidores, incluida la columna recién agregados en el `Suppliers` lista de columnas de la tabla s. Además, se ajustará automáticamente la expresión escrita en el cuadro de texto (fórmula) en una expresión equivalente que quita los espacios en blanco innecesarios, rodea los nombres de columna entre corchetes (`[]`) e incluye los paréntesis para mostrar más explícitamente el orden de operaciones:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Para obtener más información sobre las columnas calculadas de Microsoft SQL Server, consulte el [documentación técnica](https://msdn.microsoft.com/library/ms191250.aspx). Consulte también la [Cómo: Specify Computed Columns](https://msdn.microsoft.com/library/ms188300.aspx) para ver un tutorial paso a paso de creación de columnas calculadas.

> [!NOTE]
> De forma predeterminada, las columnas calculadas no se almacenan físicamente en la tabla, pero en su lugar, se vuelven a calcular cada vez que se hace referencia en una consulta. Activando la casilla de verificación se conserva, sin embargo, puede indicar a SQL Server para almacenar físicamente la columna calculada en la tabla. Esto permite que un índice que se crearán en la columna calculada, lo que puede mejorar el rendimiento de las consultas que utilizan el valor de columna calculada en sus `WHERE` cláusulas. Vea [crear índices en columnas calculadas](https://msdn.microsoft.com/library/ms189292.aspx) para obtener más información.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Paso 2: Ver los s valores de columna calculada

Antes de que se inicia el trabajo en la capa de acceso a datos, permiten s Tómese un minuto para ver el `FullContactName` valores. Desde el Explorador de servidores, haga doble clic en el `Suppliers` nombre de la tabla y elija nueva consulta en el menú contextual. Se abrirá una ventana de consulta que se le solicita que elija qué tablas se deben incluir en la consulta. Agregue el `Suppliers` de tabla y haga clic en Cerrar. A continuación, compruebe el `CompanyName`, `ContactName`, `ContactTitle`, y `FullContactName` las columnas de la tabla Suppliers. Por último, haga clic en el icono de exclamación rojo en la barra de herramientas para ejecutar la consulta y ver los resultados.

Como se muestra en la figura 2, los resultados incluyen `FullContactName`, que muestra la `CompanyName`, `ContactName`, y `ContactTitle` columnas mediante la sección formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![El FullContactName utiliza formato ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: el `FullContactName` utiliza el formato `ContactName` (`ContactTitle`, `CompanyName`) ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Paso 3: Agregar la`SuppliersTableAdapter`a la capa de acceso a datos

Para poder trabajar con la información de proveedor en nuestra aplicación es necesario crear primero un TableAdapter y DataTable en nuestro DAL. Idealmente, esto se lograría siguiendo los mismos pasos sencillos examinados en tutoriales anteriores. Sin embargo, trabajar con columnas calculadas presenta unas arrugas que merecen explicación.

Si usas un TableAdapter que utiliza instrucciones SQL ad hoc, simplemente puede incluir la columna calculada en la consulta principal de TableAdapter s mediante el Asistente para configuración de TableAdapter. Esto, sin embargo, generará automáticamente `INSERT` y `UPDATE` las instrucciones que incluyen la columna calculada. Si intenta ejecutar uno de estos métodos, un `SqlException` con el mensaje de la columna *ColumnName* no se puede modificar porque es una columna calculada o es el resultado de un operador UNION se iniciará. Mientras el `INSERT` y `UPDATE` instrucción se puede ajustar manualmente a través de las operaciones de asignación TableAdapter `InsertCommand` y `UpdateCommand` propiedades, estas personalizaciones se perderán cuando se vuelve a ejecutar el Asistente para configuración de TableAdapter.

Debido a la fragilidad de los TableAdapter utilizan instrucciones SQL ad hoc, se recomienda que utilizamos procedimientos almacenados cuando se trabaja con columnas calculadas. Si está utilizando procedimientos almacenados existentes, simplemente configure TableAdapter como se describe en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial. Sin embargo, si tiene el Asistente de TableAdapter para crear los procedimientos almacenados para usted, es importante inicialmente omitir las columnas calculadas de la consulta principal. Si incluye una columna calculada en la consulta principal, el Asistente para configuración de TableAdapter le informará, tras la finalización, que no puede crear los procedimientos almacenados correspondientes. En resumen, es necesario configurar inicialmente el TableAdapter utilizando una consulta principal libre de columnas calculada y, a continuación, actualizar manualmente el procedimiento almacenado correspondiente y los TableAdapter s `SelectCommand` para incluir la columna calculada. Este enfoque es similar al utilizado en el [actualizar TableAdapter que se utilizan](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* tutorial.

Para este tutorial, permiten s Agregar un nuevo TableAdapter y hacer que se cree automáticamente los procedimientos almacenados para nosotros. Por lo tanto, tendrá que inicialmente se omite la `FullContactName` columna calculada de la consulta principal.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos en el `~/App_Code/DAL` carpeta. Haga clic en el diseñador y, en el menú contextual, elija Agregar un nuevo TableAdapter. Se iniciará al Asistente para configuración de TableAdapter. Especifique la base de datos para consultar los datos desde (`NORTHWNDConnectionString` de `Web.config`) y haga clic en siguiente. Puesto que aún no hemos creado los procedimientos almacenados para consultar o modificar el `Suppliers` tabla, seleccione crear nuevos procedimientos almacenados de opción para que el asistente los cree nos y haga clic en siguiente.


[![Elija el crear nuevos procedimientos almacenados opción](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: elija el crear nuevos procedimientos almacenados opción ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image9.png))


El paso posterior nos pide la consulta principal. Escriba la siguiente consulta, que devuelve el `SupplierID`, `CompanyName`, `ContactName`, y `ContactTitle` columnas para cada proveedor. Tenga en cuenta que esta consulta intencionadamente omite la columna calculada (`FullContactName`); se actualizará el procedimiento almacenado correspondiente para incluir esta columna en el paso 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Después de escribir la consulta principal y hacer clic en siguiente, el asistente permite los cuatro procedimientos almacenados que generará el nombre. El nombre de estos procedimientos almacenados `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, y `Suppliers_Delete`, como se muestra en la figura 4.


[![Personalizar los nombres de los procedimientos almacenados generados automáticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizar los nombres de los procedimientos almacenados de Auto-Generated ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image12.png))


El siguiente paso del asistente permite asignar un nombre de los métodos de TableAdapter s y especificar los patrones que se usan para obtener acceso y actualizar datos. Deje todas las casillas de tres verificación activadas, pero cambie el nombre de la `GetData` método `GetSuppliers`. Haga clic en Finalizar para completar al asistente.


[![Cambiar el nombre de método GetData GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: cambiar el nombre de la `GetData` método `GetSuppliers` ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image15.png))


Al hacer clic en Finalizar, el asistente creará los cuatro procedimientos almacenados y agregar el TableAdapter y la tabla correspondiente al conjunto de datos con tipo.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Paso 4: Incluida la columna calculada en la consulta principal de TableAdapter s

Ahora necesitamos actualizar el TableAdapter y DataTable creado en el paso 3 para incluir la `FullContactName` columna calculada. Esto implica dos pasos:

1. Actualizando el `Suppliers_Select` procedimiento almacenado para devolver el `FullContactName` una columna calculada, y
2. Actualizar la tabla de datos para incluir correspondiente `FullContactName` columna.

Iniciar, vaya al explorador de servidores y explorar en profundidad en la carpeta procedimientos almacenados. Abra la `Suppliers_Select` procedimiento almacenado y actualización de la `SELECT` consulta incluya el `FullContactName` columna calculada:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Guarde los cambios en el procedimiento almacenado, haga clic en el icono Guardar en la barra de herramientas, presionando Ctrl + S o eligiendo la operación de guardar `Suppliers_Select` opción en el menú archivo.

A continuación, vuelva al diseñador de DataSet, haga doble clic en el `SuppliersTableAdapter`y elija Configurar en el menú contextual. Tenga en cuenta que la `Suppliers_Select` columna ahora incluye la `FullContactName` columna en su colección de columnas de datos.


[![Ejecutar el Asistente para configuración de TableAdapter s para actualizar las columnas de s de DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: ejecutar el Asistente para configuración para actualizar las columnas de la asignación de DataTable de TableAdapter s ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image18.png))


Haga clic en Finalizar para completar al asistente. Esto agregará automáticamente una columna correspondiente a la `SuppliersDataTable`. El Asistente de TableAdapter es lo suficientemente inteligente como para detectar que la `FullContactName` columna es una columna calculada y, por tanto, de sólo lectura. Por lo tanto, Establece la columna s `ReadOnly` propiedad `true`. Para comprobarlo, seleccione la columna de la `SuppliersDataTable` y, a continuación, vaya a la ventana Propiedades (consulte la figura 7). Tenga en cuenta que la `FullContactName` columna s `DataType` y `MaxLength` propiedades también se establecen en consecuencia.


[![La columna FullContactName está marcada como de solo lectura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: el `FullContactName` columna está marcada como de sólo lectura ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Paso 5: Agregar un`GetSupplierBySupplierID`método a TableAdapter

Para este tutorial crearemos una página ASP.NET que muestra los proveedores en una cuadrícula actualizable. En más allá de los tutoriales hemos actualizado un único registro de la capa de lógica de negocios mediante la recuperación que copia de registro concreto de la capa DAL como un DataTable fuertemente tipado, actualizar sus propiedades y, a continuación, enviar la tabla de datos actualizada en la capa DAL para propagar los cambios a la base de datos. Para lograr este primer paso - recuperar del registro que se va a actualizar desde la capa DAL - que necesitamos agregar primero un `GetSupplierBySupplierID(supplierID)` método a la capa DAL.

Haga doble clic en el `SuppliersTableAdapter` en el diseño de conjunto de datos y elija la opción de Agregar consulta en el menú contextual. Igual que hicimos en el paso 3, permiten el Asistente para generar un nuevo procedimiento almacenado para que podamos seleccionando la opción de crear nuevo procedimiento almacenado (hacen referencia a la figura 3 para una captura de pantalla de este paso del asistente). Ya que este método devolverá un registro con varias columnas, indicar que desea utilizar una consulta SQL que es una instrucción SELECT que devuelve filas y haga clic en siguiente.


[![Elija la selección que devuelve filas, opción](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: elija la selección que devuelve filas opción ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image24.png))


El paso posterior solicita us para la consulta que se utilizará para este método. Escriba lo siguiente, que devuelve los mismos campos de datos como la consulta principal, pero para un proveedor determinado.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

La siguiente pantalla nos pide el nombre del procedimiento almacenado que se va a generar automáticamente. Nombre de este procedimiento almacenado `Suppliers_SelectBySupplierID` y haga clic en siguiente.


[![Nombre del procedimiento almacenado Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nombre del procedimiento almacenado `Suppliers_SelectBySupplierID` ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image27.png))


Por último, las indicaciones del Asistente para los datos de acceso a patrones de nombres y los métodos que se usará para el objeto TableAdapter. Deje las casillas de verificación activadas, pero cambie el nombre de la `FillBy` y `GetDataBy` métodos a `FillBySupplierID` y `GetSupplierBySupplierID`, respectivamente.


[![Nombre de la FillBySupplierID de métodos de TableAdapter y GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: el nombre de los métodos de TableAdapter `FillBySupplierID` y `GetSupplierBySupplierID` ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image30.png))


Haga clic en Finalizar para completar al asistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Paso 6: Crear la capa de lógica de negocios

Antes de crear una página ASP.NET que utiliza la columna calculada que creó en el paso 1, primero se debe agregar los métodos correspondientes en la capa BLL. Nuestra página de ASP.NET, lo cual se creará en el paso 7, permitirá a los usuarios ver y editar los proveedores. Por lo tanto, tenemos nuestro BLL para proporcionar, como mínimo, un método para obtener todos los proveedores y otro para actualizar un proveedor determinado.

Cree un nuevo archivo de clase denominado `SuppliersBLLWithSprocs` en el `~/App_Code/BLL` carpeta y agregue el código siguiente:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Al igual que las demás clases BLL `SuppliersBLLWithSprocs` tiene un `protected` `Adapter` propiedad que devuelve una instancia de la `SuppliersTableAdapter` clase junto con dos `public` métodos: `GetSuppliers` y `UpdateSupplier`. El `GetSuppliers` método se llama y se devuelve el `SuppliersDataTable` devuelto por el correspondiente `GetSupplier` método en la capa de acceso a datos. El `UpdateSupplier` método recupera información sobre el proveedor determinado que se está actualizando a través de una llamada a la capa DAL s `GetSupplierBySupplierID(supplierID)` método. A continuación, actualiza el `CategoryName`, `ContactName`, y `ContactTitle` propiedades y confirma estos cambios en la base de datos mediante una llamada a la capa de acceso a datos s `Update` método, pasando modificados `SuppliersRow` objeto.

> [!NOTE]
> Excepto para `SupplierID` y `CompanyName`, permitir que todas las columnas de la tabla Suppliers `NULL` valores. Por lo tanto, si en el pasado `contactName` o `contactTitle` parámetros son `null` es necesario establecer correspondiente `ContactName` y `ContactTitle` propiedades para un `NULL` valor de base de datos mediante el `SetContactNameNull` y `SetContactTitleNull`métodos, respectivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Paso 7: Trabajar con la columna calculada de la capa de presentación

Con la columna calculada que se agrega a la `Suppliers` tabla y la capa DAL y BLL se actualizan en consecuencia, estamos preparados crear una página ASP.NET que funciona con la `FullContactName` columna calculada. Comience abriendo la `ComputedColumns.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Establecer la s GridView `ID` propiedad `Suppliers` y, en su etiqueta inteligente, enlazarla a un nuevo ObjectDataSource denominado `SuppliersDataSource`. Configurar el ObjectDataSource para usar la `SuppliersBLLWithSprocs` clase agregamos copia en el paso 6 y haga clic en siguiente.


[![Configurar el ObjectDataSource para utilizar la clase SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurar el ObjectDataSource que se utilizan los `SuppliersBLLWithSprocs` clase ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image33.png))


Hay solo dos métodos definidos en el `SuppliersBLLWithSprocs` clase: `GetSuppliers` y `UpdateSupplier`. Asegúrese de que estos dos métodos se especifican en la instrucción SELECT y actualización pestañas, respectivamente y haga clic en Finalizar para completar la configuración del origen ObjectDataSource.

Cuando se complete el Asistente para configuración de origen de datos, Visual Studio agregará un BoundField para cada uno de los campos de datos devueltos. Quitar el `SupplierID` BoundField y cambie la `HeaderText` propiedades de la `CompanyName`, `ContactName`, `ContactTitle`, y `FullContactName` BoundFields para la empresa, póngase en contacto con nombre, título y nombre completo del contacto, respectivamente. Desde la etiqueta inteligente, active la casilla Habilitar edición para activar las capacidades de edición de GridView s integradas.

Además de agregar BoundFields a GridView, complete el Asistente para orígenes de datos también hace que Visual Studio establecer la s ObjectDataSource `OldValuesParameterFormatString` propiedad original\_{0}. Revertir esta configuración en su valor predeterminado, {0}.

Después de realizar estas modificaciones en el control GridView y ObjectDataSource, su marcado declarativo debe ser similar al siguiente:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

A continuación, visite esta página a través de un explorador. Como se muestra en la figura 12, cada proveedor se muestra en una cuadrícula que incluye el `FullContactName` formato de columna, cuyo valor es simplemente concatenación de las otras tres columnas `ContactName` (`ContactTitle`, `CompanyName`).


[![Cada proveedor se muestra en la cuadrícula](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: cada proveedor se muestra en la cuadrícula ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image36.png))


Haga clic en el botón Editar para un proveedor determinado provoca una devolución de datos y se esa fila representado en su edición de la interfaz (Véase la figura 13). Las tres primeras columnas se representan en su interfaz de edición predeterminado: control de un cuadro de texto cuya `Text` propiedad está establecida en el valor del campo de datos. La `FullContactName` columna, sin embargo, no como texto. Cuando el BoundFields se agregaron a la GridView al finalizar el Asistente para la configuración del origen de datos, el `FullContactName` BoundField s `ReadOnly` propiedad se estableció en `true` porque correspondiente `FullContactName` columna en el `SuppliersDataTable` tiene su `ReadOnly` propiedad establecida en `true`. Como se indicó en el paso 4, el `FullContactName` s `ReadOnly` propiedad se estableció en `true` porque el TableAdapter detectó que la columna es una columna calculada.


[![La columna FullContactName no es modificables](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: el `FullContactName` columna no es modificables ([haga clic aquí para ver la imagen a tamaño completo](working-with-computed-columns-cs/_static/image39.png))


Continúe y actualizar el valor de una o varias de las columnas que se puede editables y haga clic en actualizar. Tenga en cuenta cómo el `FullContactName` valor s se actualiza automáticamente para reflejar el cambio.

> [!NOTE]
> GridView utiliza actualmente BoundFields para los campos editables, lo que da lugar a la interfaz de edición predeterminado. Puesto que la `CompanyName` campo es obligatorio, se debe convertir en TemplateField que incluye un control RequiredFieldValidator. Dejar este como un ejercicio para el lector interesado. Consulte la [agregar controles de validación a las Interfaces de insertar y modificar](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para obtener instrucciones paso a paso sobre convertir BoundField a TemplateField y agregar controles de validación.


## <a name="summary"></a>Resumen

Al definir el esquema de una tabla, Microsoft SQL Server permite la inclusión de las columnas calculadas. Se trata de columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas en el mismo registro. Desde los valores para las columnas calculadas se basan en una expresión, que son de solo lectura y no se puede asignar un valor en un `INSERT` o `UPDATE` instrucción. Esto presenta desafíos cuando se usa una columna calculada en la consulta principal de un TableAdapter que intenta generar automáticamente correspondiente `INSERT`, `UPDATE`, y `DELETE` las instrucciones.

En este tutorial se describen técnicas para sortear los desafíos que suponen las columnas calculadas. En concreto, usamos los procedimientos almacenados en nuestro TableAdapter para superar la fragilidad inherente en un TableAdapter que utilizan instrucciones SQL ad hoc. Al tener el Asistente de TableAdapter para crear nuevos procedimientos almacenados, es importante que tenemos la consulta principal inicialmente omitir las columnas calculadas porque su presencia evita que los procedimientos almacenados de modificación de datos que se está generando. Una vez configurado inicialmente el TableAdapter, su `SelectCommand` procedimiento almacenado puede ser reformó para incluir las columnas calculadas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Hilton Geisenow y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-additional-datatable-columns-cs.md)
> [Siguiente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
