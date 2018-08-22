---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Actualizar TableAdapter para usar Join (C#) | Microsoft Docs
author: rick-anderson
description: Cuando se trabaja con una base de datos es común a los datos de solicitud que se reparten entre varias tablas. Para recuperar datos de dos tablas diferentes podemos usar ya sea...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: d5b69b502cf650a477b2841c25ad3f1ab5f8da93
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827804"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Actualizar TableAdapter para usar Join (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) o [descargar PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Cuando se trabaja con una base de datos es común a los datos de solicitud que se reparten entre varias tablas. Podemos usar una subconsulta correlacionada o una operación de combinación para recuperar datos de dos tablas diferentes. En este tutorial se compare subconsultas correlacionadas y la sintaxis de combinación antes de ver cómo crear un TableAdapter que incluye una combinación de la consulta principal.


## <a name="introduction"></a>Introducción

Con bases de datos relacionales los datos que está interesados en trabajar con frecuencia se reparten entre varias tablas. Por ejemplo, al mostrar información de producto es probable que queremos mostrar cada categoría de producto s correspondiente y los nombres de proveedor s. El `Products` tabla tiene `CategoryID` y `SupplierID` valores, pero los nombres de categoría y el proveedor reales se encuentran en el `Categories` y `Suppliers` tablas, respectivamente.

Para recuperar información de otra tabla relacionada, se puede usar *subconsultas correlacionadas* o `JOIN` *s*. Una subconsulta correlacionada está anidada `SELECT` consulta que hace referencia a columnas de la consulta externa. Por ejemplo, en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial hemos usado dos subconsultas correlacionadas en el `ProductsTableAdapter` consulta principal de s para devolver los nombres de categoría y el proveedor para cada producto. Un `JOIN` es una construcción SQL que combina las filas relacionadas de dos tablas diferentes. Hemos usado un `JOIN` en el [consultar datos con el SqlDataSource Control](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) tutorial para mostrar información de categoría junto con cada producto.

El motivo nos hemos abstención utilizasen `JOIN` s con los TableAdapters es debido a limitaciones en el Asistente para la s de TableAdapter para generar automáticamente correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones. Más concretamente, si la consulta principal de TableAdapter s contiene cualquier `JOIN` s, TableAdapter no puede crear automáticamente las instrucciones SQL ad hoc o los procedimientos almacenados para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades.

En este tutorial se comparará brevemente y contraste las subconsultas correlacionadas y `JOIN` s antes de explorar cómo crear un TableAdapter que incluye `JOIN` s en su consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparar y contrastar las subconsultas correlacionadas y`JOIN` s

Recuerde que el `ProductsTableAdapter` creado en el primer tutorial de la `Northwind` conjunto de datos utiliza las subconsultas correlativas para devolver cada nombre producto s de categoría y el proveedor correspondiente. El `ProductsTableAdapter` consulta principal s se muestra a continuación.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Los dos correlacionado subconsultas - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` y `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -son `SELECT` consultas que devuelven un único valor por el producto como una columna adicional en el exterior `SELECT` lista de columnas de la instrucción s.

Como alternativa, un `JOIN` puede usarse para devolver cada nombre de proveedor y categoría de producto s. La siguiente consulta devuelve el mismo resultado que lo anterior, pero utiliza `JOIN` s en lugar de subconsultas:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Un `JOIN` combina los registros de una tabla con registros de otra tabla según algunos criterios. En la consulta anterior, por ejemplo, el `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica a SQL Server para combinar cada registro del producto con la categoría de registro cuya `CategoryID` valor coincide con el producto s `CategoryID` valor. El resultado combinado nos permite trabajar con los campos de categoría correspondiente para cada producto (como `CategoryName`).

> [!NOTE]
> `JOIN` s se usan normalmente cuando se consultan datos desde bases de datos relacionales. Si está familiarizado con el `JOIN` sintaxis o necesidad repasarlo un poco acerca de su uso, d recomiendo el [SQL Join tutorial](http://www.w3schools.com/sql/sql_join.asp) en [W3 Schools](http://www.w3schools.com/). También vale la pena leer son la [ `JOIN` Fundamentos](https://msdn.microsoft.com/library/ms191517.aspx) y [conceptos básicos de subconsultas](https://msdn.microsoft.com/library/ms189575.aspx) secciones de la [libros en pantalla de SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Puesto que `JOIN` s y subconsultas correlacionadas ambos sirve para recuperar datos relacionados de otras tablas, muchos desarrolladores quedan arañazos sus encabezados y se pregunta qué aproximación utilizar. Todos los gurús de SQL se ve hablado han dicho aproximadamente lo mismo que TI t importa realmente en cuanto al rendimiento como SQL Server generará los planes de ejecución casi idénticos. A continuación, sus consejos, es usar la técnica que usted y su equipo se sienta más cómodos. Merece teniendo en cuenta que después de transmitir este Consejo estos expertos inmediatamente expresan sus preferencias de `JOIN` s a lo largo de las subconsultas correlativas.

Cuando se crea una capa de acceso a datos mediante conjuntos de datos con tipo, las herramientas funcionan mejor cuando el uso de subconsultas. En concreto, el Asistente para la s de TableAdapter no generará automáticamente correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones si la consulta principal contiene cualquier `JOIN` s, pero generará automáticamente estas instrucciones cuando están correlacionados subconsultas se usan.

Para explorar este inconveniente, crear un conjunto de datos de tipo temporal en el `~/App_Code/DAL` carpeta. Durante el Asistente para configuración de TableAdapter, optar por usar instrucciones SQL ad hoc y escriba lo siguiente `SELECT` consulta (consulte la figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Escriba una consulta principal que contiene las combinaciones](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Figura 1**: escriba una consulta principal que contiene `JOIN` s ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


De forma predeterminada, se creará automáticamente el TableAdapter `INSERT`, `UPDATE`, y `DELETE` las instrucciones en función de la consulta principal. Si hace clic en el botón Opciones avanzadas, que puede ver que esta característica está habilitada. A pesar de esta configuración, lo TableAdapter no podrá crear el `INSERT`, `UPDATE`, y `DELETE` instrucciones porque la consulta principal contiene un `JOIN`.


![Escriba una consulta principal que contiene las combinaciones](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Figura 2**: escriba una consulta principal que contenga `JOIN` s


Haga clic en Finalizar para completar al asistente. En este momento su diseñador de DataSet s incluirá un solo TableAdapter con un objeto DataTable con columnas para cada uno de los campos devueltos en el `SELECT` lista de columnas de la consulta s. Esto incluye la `CategoryName` y `SupplierName`, como se muestra en la figura 3.


![La tabla de datos incluye una columna para cada campo devuelto en la lista de columnas](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Figura 3**: la tabla de datos incluye una columna para cada campo se devuelve en la lista de columnas


Aunque la tabla de datos tiene las columnas apropiadas, TableAdapter le faltan valores para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. Para confirmarlo, haga clic en el TableAdapter en el diseñador y, a continuación, vaya a la ventana Propiedades. Allí verá el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades se establecen en (None).


[![El InsertCommand, UpdateCommand y DeleteCommand propiedades se establecen en (None)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Figura 4**: el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades se establecen en (None) ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Para evitar este inconveniente, podemos proporcionar manualmente las instrucciones SQL y parámetros para el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades a través de la ventana Propiedades. Como alternativa, podríamos comenzar mediante la configuración de la consulta principal de TableAdapter s a *no* incluir cualquiera `JOIN` s. Esto permitirá que el `INSERT`, `UPDATE`, y `DELETE` instrucciones al generarse automáticamente para nosotros. Después de completar el asistente, nos podríamos, a continuación, actualizar manualmente los TableAdapter s `SelectCommand` desde la ventana Propiedades, por lo que incluye el `JOIN` sintaxis.

Aunque este enfoque funciona, es muy complicado cuando el uso de consultas SQL ad hoc porque cada vez que la consulta principal de TableAdapter s es volver a configurar mediante el asistente, el generado automáticamente `INSERT`, `UPDATE`, y `DELETE` se vuelven a crear instrucciones. Esto significa que todas las personalizaciones se realizan más adelante se perderían si de había hecho en el TableAdapter, ha elegido configurar en el menú contextual y completar al Asistente para nuevo.

La fragilidad de las operaciones de asignación TableAdapter generadas automáticamente `INSERT`, `UPDATE`, y `DELETE` instrucciones es, Afortunadamente, limitada a instrucciones SQL ad hoc. Si el TableAdapter utiliza los procedimientos almacenados, puede personalizar el `SelectCommand`, `InsertCommand`, `UpdateCommand`, o `DeleteCommand` procedimientos almacenados y vuelva a ejecutar el Asistente para configuración de TableAdapter sin necesidad de miedo que será los procedimientos almacenados puede modificar.

En los próximos varios pasos que se creará un TableAdapter que, inicialmente, utiliza una consulta principal que omite cualquier `JOIN` s para que el correspondiente a insertar, actualizar y procedimientos almacenados de eliminación será generado automáticamente. A continuación, actualizaremos la `SelectCommand` hasta que se usa un `JOIN` que devuelve más columnas de las tablas relacionadas. Por último, crearemos una clase correspondiente de la capa de lógica empresarial y muestran cómo utilizar el TableAdapter en una página web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Paso 1: Creación de TableAdapter mediante una consulta principal simplificada

En este tutorial se agregará a un TableAdapter y DataTable fuertemente tipados para la `Employees` tabla el `NorthwindWithSprocs` conjunto de datos. El `Employees` tabla contiene un `ReportsTo` campo que especifica el `EmployeeID` del Administrador de empleado s. Por ejemplo, los empleados Anne Dodsworth tiene un `ReportTo` valor de 5, que es el `EmployeeID` de Steven Buchanan. Por lo tanto, Anne informa a Steven, su administrador. Junto con informes de cada empleado s `ReportsTo` valor, también deseamos recuperar el nombre de su jefe. Esto puede realizarse mediante un `JOIN`. Pero al usar un `JOIN` al crear inicialmente el TableAdapter impide que el Asistente para generar automáticamente la inserción correspondiente, actualizar y eliminar las capacidades. Por lo tanto, comenzaremos por crear un objeto TableAdapter cuya consulta principal no contiene ningún `JOIN` s. A continuación, en el paso 2, actualizaremos el procedimiento almacenado la consulta principal para recuperar el nombre del administrador s a través de un `JOIN`.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos en el `~/App_Code/DAL` carpeta. Haga doble clic en el diseñador, seleccione la opción Agregar en el menú contextual y elegir el elemento de menú de TableAdapter. Esto iniciará al Asistente para configuración de TableAdapter. Tal y como se muestra en la figura 5, que el asistente cree nuevos procedimientos almacenados y haga clic en siguiente. Para hacer un repaso sobre la creación de nuevos procedimientos almacenados desde el Asistente para la s de TableAdapter, consulte el [crear procedimientos almacenados nuevo para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial.


[![Seleccione los procedimientos almacenados nueva opción de crear](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Figura 5**: seleccione Create new almacenados procedures (opción) ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Use el siguiente `SELECT` instrucción para la consulta principal de TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Puesto que esta consulta no incluye ningún `JOIN` s, el Asistente de TableAdapter creará automáticamente los procedimientos almacenados con el correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones, así como un procedimiento almacenado para ejecutar el método main consulta.

El siguiente paso nos permite nombrar los procedimientos almacenados de TableAdapter. Use los nombres `Employees_Select`, `Employees_Insert`, `Employees_Update`, y `Employees_Delete`, como se muestra en la figura 6.


[![Nombre de los procedimientos almacenados de TableAdapter](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Figura 6**: el nombre de los procedimientos almacenados de TableAdapter s ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


El último paso nos pedirá un nombre de los métodos de TableAdapter s. Use `Fill` y `GetEmployees` como los nombres de método. Además, asegúrese de dejar el crear métodos para enviar actualizaciones directamente a la casilla de la base de datos (GenerateDBDirectMethods) activada.


[![Nombre del relleno de los métodos de TableAdapter s y GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Figura 7**: nombre de los métodos de TableAdapter s `Fill` y `GetEmployees` ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Después de completar al asistente, dedique un momento a examinar los procedimientos almacenados en la base de datos. Debería ver cuatro nuevas: `Employees_Select`, `Employees_Insert`, `Employees_Update`, y `Employees_Delete`. A continuación, inspeccione el `EmployeesDataTable` y `EmployeesTableAdapter` acaba de crear. La tabla de datos contiene una columna para cada campo devuelto por la consulta principal. Haga clic en el TableAdapter y, a continuación, vaya a la ventana Propiedades. Allí verá el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades están configuradas correctamente para llamar a los procedimientos almacenados correspondientes.


[![El TableAdapter incluye Insertar, actualizar y eliminar las capacidades](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Figura 8**: The TableAdapter incluye Insert, Update y eliminar capacidades ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Al insertar, actualizar y eliminar procedimientos almacenados que se crean automáticamente y la `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades configuradas correctamente, estamos preparados para personalizar el `SelectCommand` s procedimiento almacenado para devolver adicionales información acerca de cada empleado s Administrador. En concreto, deberá actualizar el `Employees_Select` procedimiento almacenado para usar un `JOIN` y devolver el Administrador de s `FirstName` y `LastName` valores. Después de que se ha actualizado el procedimiento almacenado, necesitamos actualizar la tabla de datos para que incluya estas columnas adicionales. Trataremos estas dos tareas en los pasos 2 y 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Paso 2: Personalizar el procedimiento almacenado para incluir un`JOIN`

Empiece por ir en el Explorador de servidores, profundizar en la carpeta de procedimientos almacenados de s de la base de datos Northwind y abrir el `Employees_Select` procedimiento almacenado. Si no ve este procedimiento almacenado, haga doble clic en la carpeta procedimientos almacenados y elija actualizar. Actualizar el procedimiento almacenado para que utilice un `LEFT JOIN` para devolver el Administrador de s primero el nombre y apellido:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Después de actualizar el `SELECT` instrucción, guarde los cambios, vaya al menú archivo y elegir guardar `Employees_Select`. Como alternativa, puede haga clic en el icono Guardar en la barra de herramientas o presione CTRL+s. Después de guardar los cambios, haga doble clic en el `Employees_Select` procedimiento almacenado en el Explorador de servidores y elija ejecutar. Esto ejecutará el procedimiento almacenado y mostrar sus resultados en la ventana de salida (consulte la figura 9).


[![Los resultados de los procedimientos almacenados se muestran en la ventana de salida](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Figura 9**: los resultados de procedimientos almacenados se muestran en la ventana de salida ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Paso 3: Actualizar las columnas de DataTable s

En este momento, el `Employees_Select` procedimiento almacenado devuelve `ManagerFirstName` y `ManagerLastName` valores, pero la `EmployeesDataTable` no tiene estas columnas. Estas columnas que faltan pueden agregarse a la DataTable de dos maneras:

- **Manualmente** : haga doble clic en la DataTable en el Diseñador de DataSet y, en el menú Agregar, elija la columna. Puede, a continuación, la columna de nombre y establezca sus propiedades según corresponda.
- **Automáticamente** -el Asistente para configuración de TableAdapter actualizará las columnas de DataTable s para reflejar los campos devueltos por la `SelectCommand` procedimiento almacenado. Al usar instrucciones SQL ad hoc, el asistente también quitará el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades desde el `SelectCommand` contiene ahora un `JOIN`. Pero cuando se utilizan procedimientos almacenados, estas propiedades de comando permanecen intactas.

Agregar manualmente las columnas de DataTable se han analizado en los tutoriales anteriores, incluidos [maestro/detalle con una lista con viñetas de registros maestros con un control DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) y [cargar archivos](../working-with-binary-files/uploading-files-cs.md), y le enviaremos Examine este proceso nuevo con más detalle en nuestro tutorial siguiente. Para este tutorial, sin embargo, permiten s usar el enfoque automático mediante el Asistente para configuración de TableAdapter.

Iniciar con el botón secundario en el `EmployeesTableAdapter` y seleccione Configurar en el menú contextual. Se abrirá el Asistente para configuración de TableAdapter, que enumera los procedimientos almacenados utilizados para seleccionar, insertar, actualizar y eliminar, junto con sus valores devueltos y parámetros (si existe). Figura 10 muestra a este asistente. Aquí podemos ver que el `Employees_Select` procedimiento almacenado ahora devuelve el `ManagerFirstName` y `ManagerLastName` campos.


[![Se muestra en el Asistente para la lista de columnas actualizadas para el Employees_Select de procedimiento almacenado](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Figura 10**: el asistente muestra la lista de columnas actualizado para el `Employees_Select` Stored Procedure ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Complete el asistente, haga clic en Finalizar. Al volver al diseñador de DataSet, el `EmployeesDataTable` incluye dos columnas adicionales: `ManagerFirstName` y `ManagerLastName`.


[![El EmployeesDataTable contiene dos nuevas columnas](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Figura 11**: el `EmployeesDataTable` contiene dos nuevas columnas ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Para ilustrar que la actualización `Employees_Select` procedimiento almacenado está activo y que la inserción, actualizar y eliminar las capacidades del TableAdapter siguen siendo funcionales, permiten s crear una página web que permite a los usuarios ver y eliminar los empleados. Antes de crear una página, sin embargo, debemos crear una nueva clase primero en la capa de lógica empresarial para trabajar con los empleados de la `NorthwindWithSprocs` conjunto de datos. En el paso 4, se creará un `EmployeesBLLWithSprocs` clase. En el paso 5, usamos esta clase desde una página ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Paso 4: Implementar la capa de lógica de negocios

Cree un nuevo archivo de clase en el `~/App_Code/BLL` carpeta denominada `EmployeesBLLWithSprocs.cs`. Esta clase imita la semántica de las existentes `EmployeesBLL` (clase), solo esta nueva uno proporciona menos métodos y usa el `NorthwindWithSprocs` conjunto de datos (en lugar de la `Northwind` conjunto de datos). Agregue el código siguiente a la clase `EmployeesBLLWithSprocs`.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

El `EmployeesBLLWithSprocs` clase s `Adapter` propiedad devuelve una instancia de la `NorthwindWithSprocs` s de conjunto de datos `EmployeesTableAdapter`. Esto se usa la clase s `GetEmployees` y `DeleteEmployee` métodos. El `GetEmployees` llamadas al método el `EmployeesTableAdapter` s correspondiente `GetEmployees` método, que invoca la `Employees_Select` procedimiento almacenado y rellena los resultados en un `EmployeeDataTable`. El `DeleteEmployee` llama de forma similar a método la `EmployeesTableAdapter` s `Delete` método, que invoca la `Employees_Delete` procedimiento almacenado.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Paso 5: Trabajar con los datos en la capa de presentación

Con el `EmployeesBLLWithSprocs` clase completa, que está listo para trabajar con datos de empleados a través de una página ASP.NET. Abra el `JOINs.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad a `Employees`. A continuación, en la etiqueta inteligente s GridView, enlazar la cuadrícula para un nuevo control ObjectDataSource denominado `EmployeesDataSource`.

Configurar el origen ObjectDataSource para usar el `EmployeesBLLWithSprocs` clase y, desde las pestañas de seleccionar y eliminar, asegúrese de que el `GetEmployees` y `DeleteEmployee` se seleccionan los métodos de las listas desplegables. Haga clic en Finalizar para completar la configuración ObjectDataSource.


[![Configurar el origen ObjectDataSource para usar la clase EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Figura 12**: configurar el origen ObjectDataSource que se usarán el `EmployeesBLLWithSprocs` clase ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Tiene el uso de ObjectDataSource los métodos de DeleteEmployee y GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Figura 13**: tiene el uso de ObjectDataSource el `GetEmployees` y `DeleteEmployee` métodos ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio agregará BoundField en GridView para cada uno de los `EmployeesDataTable` columnas s. Quitar todos estos BoundFields excepto `Title`, `LastName`, `FirstName`, `ManagerFirstName`, y `ManagerLastName` y cambiar el nombre de la `HeaderText` las propiedades de los cuatro últimos BoundFields apellido, nombre, nombre Manager s, y Administrador de s Last Name, respectivamente.

Para permitir a los usuarios eliminar a empleados desde esta página, necesitamos hacer dos cosas. En primer lugar, indique a GridView para proporcionar capacidades de eliminación activando la opción Habilitar eliminación desde su etiqueta inteligente. En segundo lugar, cambie la s ObjectDataSource `OldValuesParameterFormatString` propiedad del valor establecido por el ObjectDataSource wizard (`original_{0}`) en su valor predeterminado (`{0}`). Después de realizar estos cambios, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Pruebe la página visitando a través de un explorador. Como se muestra en la figura 14, mostrará la página de cada empleado y su propio nombre s manager (suponiendo que tengan uno).


[![La combinación en el Employees_Select procedimiento almacenado devuelve el nombre del administrador s](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Figura 14**: el `JOIN` en el `Employees_Select` procedimiento almacenado devuelve el Administrador de s nombre ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Al hacer clic en el botón Eliminar se inicia el flujo de trabajo eliminando que culmina en la ejecución de la `Employees_Delete` procedimiento almacenado. Sin embargo, el intento `DELETE` instrucción en el procedimiento almacenado produce un error debido a una infracción de restricción de clave externa (consulte la figura 15). En concreto, cada empleado tiene uno o más registros el `Orders` tabla, provocando la eliminación de un error.


[![Eliminación de un empleado que tiene resultados pedidos correspondientes a una infracción de restricción de clave externa](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Figura 15**: eliminación de un empleado que tiene resultados pedidos correspondientes a una infracción de restricción de clave externa ([haga clic aquí para ver imagen en tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Para permitir que un empleado elimina, puede:

- Actualización de la restricción de clave externa en cascada eliminaciones,
- Elimine manualmente los registros desde el `Orders` tabla para los empleados que desea eliminar, o
- Actualización de la `Employees_Delete` procedimiento almacenado para eliminar los registros relacionados de la `Orders` tabla antes de eliminar el `Employees` registro. Analizamos esta técnica en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial.

Dejar como un ejercicio para el lector.

## <a name="summary"></a>Resumen

Cuando se trabaja con bases de datos relacionales, es habitual que las consultas extraer los datos de varias tablas relacionadas. Subconsultas correlacionadas y `JOIN` proporcionan dos técnicas distintas para tener acceso a datos de las tablas relacionadas en una consulta. En tutoriales anteriores se realizan con más frecuencia usan de subconsultas correlacionadas porque el TableAdapter no Autogenerar `INSERT`, `UPDATE`, y `DELETE` instrucciones para las consultas que implican `JOIN` s. Aunque estos valores se pueden proporcionar manualmente, al usar instrucciones SQL ad hoc se sobrescribirán las personalizaciones cuando se complete el Asistente para configuración de TableAdapter.

Afortunadamente, los TableAdapters creados mediante los procedimientos almacenados no sufren la fragilidad misma como los creados mediante instrucciones SQL ad hoc. Por lo tanto, es posible crear un TableAdapter cuya consulta principal utiliza un `JOIN` al utilizar procedimientos almacenados. En este tutorial hemos visto cómo crear este tipo de un TableAdapter. Se inició mediante un `JOIN`-menos `SELECT` de consulta para la consulta principal de TableAdapter s para que el correspondiente insert, update y procedimientos almacenados de eliminación sería creado automáticamente. TableAdapter s inicial haya completado la configuración, hemos ampliado la `SelectCommand` procedimiento almacenado para usar un `JOIN` y volver a ejecutar el Asistente de configuración de TableAdapter para actualizar el `EmployeesDataTable` columnas s.

Volver a ejecutar el Asistente de configuración de TableAdapter actualizado automáticamente el `EmployeesDataTable` columnas para reflejar los campos de datos devueltos por la `Employees_Select` procedimiento almacenado. Como alternativa, podríamos haber agregamos estas columnas manualmente a la DataTable. Exploraremos manualmente agrega columnas a la DataTable en el siguiente tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy, David Suru y Hilton Geisenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Siguiente](adding-additional-datatable-columns-cs.md)
