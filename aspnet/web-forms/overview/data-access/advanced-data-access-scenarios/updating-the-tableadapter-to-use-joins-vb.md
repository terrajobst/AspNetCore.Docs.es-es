---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Actualización de TableAdapter que se utilizan combinaciones (VB) | Documentos de Microsoft
author: rick-anderson
description: Cuando se trabaja con una base de datos es común a los datos de solicitud que se reparten entre varias tablas. Para recuperar datos de dos tablas diferentes, podemos utilizar cualquiera...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>Actualización de TableAdapter que se utilizan combinaciones (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) o [descarga de PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Cuando se trabaja con una base de datos es común a los datos de solicitud que se reparten entre varias tablas. Para recuperar datos de dos tablas diferentes podemos usar una subconsulta correlacionada o una operación de combinación. En este tutorial se compare subconsultas correlacionadas y la sintaxis de unión antes de ver cómo crear un TableAdapter que incluye una combinación en la consulta principal.


## <a name="introduction"></a>Introducción

Con bases de datos relacionales los datos con que se está interesados en trabajar a menudo se reparten entre varias tablas. Por ejemplo, al mostrar información de producto es probable que queremos mostrar cada categoría de producto s correspondiente y los nombres de proveedor s. El `Products` tabla tiene `CategoryID` y `SupplierID` valores, pero los nombres reales de categoría y el proveedor están en el `Categories` y `Suppliers` tablas, respectivamente.

Para recuperar información de otra tabla relacionada, se puede usar *subconsultas correlacionadas* o `JOIN` *s*. Una subconsulta correlacionada está anidada `SELECT` consulta que haga referencia a columnas en la consulta externa. Por ejemplo, en la [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial se utilizan dos subconsultas correlacionadas en el `ProductsTableAdapter` consulta principal de s para devolver los nombres de categoría y el proveedor para cada producto. A `JOIN` es una construcción SQL que combina las filas relacionadas de dos tablas diferentes. Se usa un `JOIN` en el [consultar datos con el SqlDataSource Control](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) tutorial para mostrar información de categorías junto a cada producto.

El motivo nos hemos abstained utilizasen `JOIN` s con los TableAdapters es debido a las limitaciones en el Asistente para la s de TableAdapter para generar automáticamente la correspondiente `INSERT`, `UPDATE`, y `DELETE` las instrucciones. Más específicamente, si la consulta principal de TableAdapter s contiene cualquier `JOIN` s, lo TableAdapter no puede crear automáticamente las instrucciones SQL ad hoc o procedimientos almacenados para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades.

En este tutorial se comparará brevemente y el contraste de las subconsultas correlacionadas y `JOIN` s antes de explorar cómo crear un TableAdapter que incluya `JOIN` s en su consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparar y contrastar las subconsultas correlacionadas y`JOIN` s

Recuerde que el `ProductsTableAdapter` creada en el primer tutorial en el `Northwind` conjunto de datos utiliza subconsultas correlativas para devolver cada nombre de categoría y el proveedor de correspondientes de un producto s. El `ProductsTableAdapter` consulta principal s se muestra a continuación.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Los dos de las subconsultas - correlacionadas `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` y `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -son `SELECT` consultas que devuelven un único valor por producto como una columna adicional en el exterior `SELECT` lista de columnas de la instrucción s.

Como alternativa, un `JOIN` puede usarse para devolver cada nombre de proveedor y de categoría de producto s. La siguiente consulta devuelve el mismo resultado que el anterior, pero usa `JOIN` s en lugar de subconsultas:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Un `JOIN` combina los registros de una tabla con los registros de otra tabla en función de algunos criterios. En la consulta anterior, por ejemplo, el `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica a SQL Server para combinar cada registro de producto con la categoría cuyo registro `CategoryID` valor coincide con el producto s `CategoryID` valor. El resultado combinado nos permite trabajar con los campos de categoría correspondiente para cada producto (como `CategoryName`).

> [!NOTE]
> `JOIN` s se usan normalmente al consultar datos de bases de datos relacionales. Si está familiarizado con la `JOIN` sintaxis o deba perfeccionar sus conocimientos un poco sobre su uso, d recomiendo el [tutorial Join de SQL](http://www.w3schools.com/sql/sql_join.asp) en [W3 varios centros escolares](http://www.w3schools.com/). También cabe lectura son el [ `JOIN` Fundamentos](https://msdn.microsoft.com/library/ms191517.aspx) y [conceptos básicos de subconsultas](https://msdn.microsoft.com/library/ms189575.aspx) secciones de la [libros en pantalla de SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Puesto que `JOIN` s y subconsultas correlacionadas se pueden usar para recuperar datos relacionados de otras tablas, muchos desarrolladores quedan arañar sus cabezales y pregunte qué aproximación utilizar. Todos los gurús de SQL se ha analizado para han dicho aproximadamente lo mismo que TI t realmente importa refiere al rendimiento cuando SQL Server generará planes de ejecución casi idénticos. Sus consejos, a continuación, consiste en utilizar la técnica que están más familiarizados con usted y su equipo. Merece teniendo en cuenta que después de transmitir estos consejos estos expertos inmediatamente expresan en sus preferencias de `JOIN` s a lo largo de las subconsultas.

Cuando se crea una capa de acceso a datos mediante conjuntos de datos con tipo, las herramientas funcionan mejor cuando se utiliza subconsultas. En concreto, el Asistente para la s de TableAdapter no generará automáticamente correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones si la consulta principal contiene cualquier `JOIN` s, pero generará automáticamente estas instrucciones cuando correlacionado subconsultas se utilizan.

Para explorar este inconveniente, cree un conjunto de datos de tipo temporal en el `~/App_Code/DAL` carpeta. Durante el Asistente para configuración de TableAdapter, decide usar instrucciones SQL ad hoc y escriba la siguiente `SELECT` consulta (consulte la figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Escriba una consulta principal que contiene las combinaciones](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figura 1**: escriba una consulta principal que contiene `JOIN` s ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


De forma predeterminada, se creará automáticamente el TableAdapter `INSERT`, `UPDATE`, y `DELETE` las instrucciones en función de la consulta principal. Si hace clic en el botón avanzadas que puede ver que esta característica está habilitada. A pesar de esta configuración, lo TableAdapter no podrá crear el `INSERT`, `UPDATE`, y `DELETE` instrucciones porque la consulta principal contiene un `JOIN`.


![Escriba una consulta principal que contiene las combinaciones](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figura 2**: escriba una consulta principal que contenga `JOIN` s


Haga clic en Finalizar para completar al asistente. En este momento su diseñador de DataSet s incluirá un TableAdapter único con un objeto DataTable con columnas para cada uno de los campos devueltos en la `SELECT` lista de columnas de la consulta s. Esto incluye la `CategoryName` y `SupplierName`, como se muestra en la figura 3.


![La tabla de datos incluye una columna para cada campo devuelto en la lista de columnas](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figura 3**: la tabla de datos incluye una columna para cada campo que se devuelve en la lista de columnas


Mientras que la tabla de datos tiene las columnas apropiadas, lo TableAdapter no tiene los valores para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. Para confirmarlo, haga clic en el objeto TableAdapter en el diseñador y, a continuación, vaya a la ventana Propiedades. No existe, verá que el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades se establecen en (ninguno).


[![El InsertCommand, UpdateCommand y DeleteCommand propiedades se establecen en (ninguno)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figura 4**: el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades se establecen en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Para solucionar este inconveniente, podemos proporcionar manualmente las instrucciones SQL y parámetros para la `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades a través de la ventana Propiedades. Como alternativa, podríamos comenzamos mediante la configuración de la consulta principal de TableAdapter s a *no* incluir cualquier `JOIN` s. Esto le permitirá la `INSERT`, `UPDATE`, y `DELETE` instrucciones que se va a generar automáticamente para que podamos. Después de completar el asistente, se pueden, a continuación, actualizar manualmente TableAdapters `SelectCommand` desde la ventana Propiedades, por lo que incluye el `JOIN` sintaxis.

Aunque este enfoque funciona, resulta muy complicado cuando el uso de consultas SQL ad hoc porque cada vez que la consulta principal de TableAdapter s es volver a configurar mediante el asistente, el generado automáticamente `INSERT`, `UPDATE`, y `DELETE` se vuelven a crear instrucciones. Esto significa que todas las personalizaciones se realizan más adelante se perderían si se con el botón secundario en TableAdapter, eligió configurar en el menú contextual y completa al Asistente para nuevo.

La fragilidad de las operaciones de asignación TableAdapter generado automáticamente `INSERT`, `UPDATE`, y `DELETE` instrucciones es, Afortunadamente, limitada a instrucciones SQL ad hoc. Si el TableAdapter utiliza los procedimientos almacenados, puede personalizar el `SelectCommand`, `InsertCommand`, `UpdateCommand`, o `DeleteCommand` procedimientos almacenados y vuelva a ejecutar el Asistente para configuración de TableAdapter sin tener que temen que será los procedimientos almacenados modificar.

En los próximos varios pasos, vamos a crear un TableAdapter que, en principio, utiliza una consulta principal que omita cualquier `JOIN` s para que los correspondientes a insertar, actualizar y procedimientos almacenados de eliminación se va a generar automáticamente. A continuación, actualizaremos la `SelectCommand` por lo tanto que utiliza un `JOIN` que devuelve más columnas de las tablas relacionadas. Por último, se creará una clase correspondiente de la capa de lógica empresarial y muestran cómo utilizar el objeto TableAdapter en una página web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Paso 1: Creación de TableAdapter utilizando una consulta principal simplificada

Para este tutorial agregaremos un TableAdapter y DataTable fuertemente tipados para la `Employees` tabla el `NorthwindWithSprocs` conjunto de datos. El `Employees` tabla contiene un `ReportsTo` campo que especifica la `EmployeeID` del Administrador de empleado s. Por ejemplo, los empleados Anne Dodsworth tiene un `ReportTo` valor de 5, que es el `EmployeeID` de Steven Buchanan. Por lo tanto, Anne informa a Carlos, su administrador. Junto con informes de cada empleado s `ReportsTo` valor, que también queramos recuperar el nombre de su jefe. Esto puede realizarse mediante un `JOIN`. Pero con un `JOIN` al crear inicialmente el TableAdapter, impide que el Asistente para generar automáticamente la inserción correspondiente, actualizar y eliminar las capacidades. Por lo tanto, se iniciará mediante la creación de un TableAdapter cuya consulta principal no contiene ninguno `JOIN` s. A continuación, en el paso 2, actualizaremos el procedimiento almacenado de consulta principal para recuperar el nombre del administrador s a través de un `JOIN`.

Comience abriendo la `NorthwindWithSprocs` conjunto de datos en el `~/App_Code/DAL` carpeta. Haga doble clic en el diseñador, seleccione la opción Agregar en el menú contextual y elija el elemento de menú de TableAdapter. Se iniciará al Asistente para configuración de TableAdapter. Tal y como se muestra en la figura 5, que el asistente cree nuevos procedimientos almacenados y haga clic en siguiente. Para repasar sobre la creación de nuevos procedimientos almacenados desde el Asistente para la s de TableAdapter, consulte el [crear procedimientos almacenados nuevos para la s de conjunto de datos con tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial.


[![Seleccione el crear nuevos procedimientos almacenados opción](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figura 5**: seleccione el crear nuevos almacenados procedures (opción) ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Utilice la siguiente `SELECT` instrucción de la consulta principal de TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Puesto que esta consulta no incluye ningún `JOIN` s, el Asistente de TableAdapter creará automáticamente los procedimientos almacenados con el correspondiente `INSERT`, `UPDATE`, y `DELETE` instrucciones, así como un procedimiento almacenado para ejecutar el método main consulta.

El siguiente paso nos permite un nombre de los procedimientos almacenados de TableAdapter. Use los nombres `Employees_Select`, `Employees_Insert`, `Employees_Update`, y `Employees_Delete`, tal y como se muestra en la figura 6.


[![Nombre de los procedimientos almacenados de TableAdapter](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figura 6**: el nombre de los procedimientos almacenados de TableAdapter s ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


El último paso le pide que asigne nombre a los métodos de TableAdapter s. Use `Fill` y `GetEmployees` como los nombres de método. Además, asegúrese de dejar el crear métodos para enviar actualizaciones directamente a la casilla de verificación de la base de datos (GenerateDBDirectMethods) activada.


[![Nombre del relleno de métodos de TableAdapter s y GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figura 7**: nombre de los métodos de TableAdapter s `Fill` y `GetEmployees` ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Después de completar al asistente, tómese un momento para examinar los procedimientos almacenados en la base de datos. Debería ver cuatro nuevos: `Employees_Select`, `Employees_Insert`, `Employees_Update`, y `Employees_Delete`. A continuación, inspeccione el `EmployeesDataTable` y `EmployeesTableAdapter` acaba de crear. La tabla de datos contiene una columna para cada campo devuelto por la consulta principal. Haga clic en el objeto TableAdapter y, a continuación, vaya a la ventana Propiedades. No existe, verá que el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades están configuradas correctamente para llamar a los procedimientos almacenados correspondientes.


[![TableAdapter incluye Insertar, actualizar y eliminar las capacidades](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figura 8**: el TableAdapter incluye Insert, Update y eliminar capacidades ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


Al insertar, actualizar y eliminar los procedimientos almacenados creados automáticamente y la `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades configurados correctamente, estamos preparados para personalizar el `SelectCommand` s procedimiento almacenado para devolver adicionales información sobre cada empleado s Administrador. En concreto, deberá actualizar el `Employees_Select` procedimiento almacenado para usar un `JOIN` y devolver el Administrador de s `FirstName` y `LastName` valores. Después de que se ha actualizado el procedimiento almacenado, necesitamos actualizar la tabla de datos para que incluya estas columnas adicionales. Abordaremos estas dos tareas en los pasos 2 y 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Paso 2: Personalizar el procedimiento almacenado debe incluir un`JOIN`

Inicio va en el Explorador de servidores, explorar en profundidad en la carpeta de procedimientos almacenados de s de la base de datos Northwind y abriendo la `Employees_Select` procedimiento almacenado. Si no ve este procedimiento almacenado, haga doble clic en la carpeta procedimientos almacenados y seleccione actualizar. Actualizar el procedimiento almacenado para que utilice un `LEFT JOIN` para devolver el administrador s primero y el apellido:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Después de actualizar el `SELECT` instrucción, guardar los cambios, vaya al menú archivo y elija Guardar `Employees_Select`. Como alternativa, puede haga clic en el icono Guardar en la barra de herramientas o aciertos CTRL+s. Después de guardar los cambios, haga doble clic en el `Employees_Select` procedimiento almacenado en el Explorador de servidores y elija ejecutar. Esto ejecutará el procedimiento almacenado y mostrar los resultados en la ventana de salida (consulte la figura 9).


[![Los resultados de los procedimientos almacenados se muestran en la ventana de salida](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figura 9**: los resultados de los procedimientos almacenados se muestran en la ventana de salida ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Paso 3: Actualizar las columnas de s de DataTable

En este momento, el `Employees_Select` procedimiento almacenado devuelve `ManagerFirstName` y `ManagerLastName` valores, pero la `EmployeesDataTable` falta estas columnas. Estas columnas que faltan pueden agregarse a la tabla de datos de una de dos maneras:

- **Manualmente** : haga doble clic en la tabla de datos en el Diseñador de DataSet y, en el menú Agregar, elija la columna. Puede, a continuación, la columna de nombre y establecer sus propiedades en consecuencia.
- **Automáticamente** -el Asistente para configuración de TableAdapter actualizará las columnas de s DataTable para reflejar los campos devueltos por la `SelectCommand` procedimiento almacenado. Al usar instrucciones SQL ad hoc, el asistente también quitará el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades desde el `SelectCommand` contiene ahora un `JOIN`. Pero cuando utilice procedimientos almacenados, estas propiedades de comando permanecen intactas.

Agregar manualmente las columnas de tabla de datos se han analizado en tutoriales anteriores, incluidos los [principal/detalle con una lista con viñetas de registros maestros DataList detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) y [cargar archivos](../working-with-binary-files/uploading-files-vb.md), y le enviaremos Buscar en este proceso nuevo con más detalle en nuestro tutorial siguiente. Para este tutorial, no obstante, permiten s, emplee el método automática mediante el Asistente para configuración de TableAdapter.

Iniciar con el botón secundario en el `EmployeesTableAdapter` y seleccionando configurar en el menú contextual. Se abrirá el Asistente para configuración de TableAdapter, que enumera los procedimientos almacenados utilizados para seleccionar, insertar, actualizar y eliminar, junto con sus valores devueltos y parámetros (si existe). La figura 10 muestra a este asistente. Aquí podemos ver que el `Employees_Select` procedimiento almacenado ahora devuelve el `ManagerFirstName` y `ManagerLastName` campos.


[![Se muestra en el Asistente para la lista de las filas actualizadas para el Employees_Select de procedimiento almacenado](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figura 10**: el asistente muestra la lista de columnas actualizado para el `Employees_Select` procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Complete el asistente, haga clic en Finalizar. Al volver al diseñador de DataSet, el `EmployeesDataTable` incluye dos columnas adicionales: `ManagerFirstName` y `ManagerLastName`.


[![El EmployeesDataTable contiene dos nuevas columnas](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figura 11**: el `EmployeesDataTable` contiene dos nuevas columnas ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Para ilustrar el hecho de que la actualización `Employees_Select` procedimiento almacenado está activo y que la inserción, actualización y eliminación de las capacidades del TableAdapter siguen siendo funcionales, permiten s crear una página web que permite a los usuarios ver y eliminar los empleados. Antes de crear una página de este tipo, sin embargo, es necesario crear primero una nueva clase en la capa de lógica de negocios para trabajar con los empleados de la `NorthwindWithSprocs` conjunto de datos. En el paso 4, crearemos un `EmployeesBLLWithSprocs` clase. En el paso 5, se usará esta clase a partir de una página ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Paso 4: Implementación de la capa de lógica de negocios

Crear un nuevo archivo de clase en el `~/App_Code/BLL` carpeta denominada `EmployeesBLLWithSprocs.vb`. Esta clase imita la semántica de las existentes `EmployeesBLL` (clase), solo esta nueva uno proporciona métodos menos y utiliza el `NorthwindWithSprocs` conjunto de datos (en lugar de la `Northwind` conjunto de datos). Agregue el código siguiente a la clase `EmployeesBLLWithSprocs`.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

El `EmployeesBLLWithSprocs` clase s `Adapter` propiedad devuelve una instancia de la `NorthwindWithSprocs` s de conjunto de datos `EmployeesTableAdapter`. Se utiliza la clase s `GetEmployees` y `DeleteEmployee` métodos. El `GetEmployees` llamadas al método el `EmployeesTableAdapter` s correspondiente `GetEmployees` método, que se invoca el `Employees_Select` procedimiento almacenado y rellena los resultados en un `EmployeeDataTable`. El `DeleteEmployee` método se llama de forma similar el `EmployeesTableAdapter` s `Delete` método, que se invoca el `Employees_Delete` procedimiento almacenado.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Paso 5: Trabajar con los datos en el nivel de presentación

Con el `EmployeesBLLWithSprocs` clase completa, se re listo para trabajar con datos de empleados a través de una página ASP.NET. Abra el `JOINs.aspx` página en el `AdvancedDAL` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Employees`. A continuación, en la etiqueta inteligente de GridView s, enlazar la cuadrícula para un nuevo control ObjectDataSource denominado `EmployeesDataSource`.

Configurar el ObjectDataSource para usar el `EmployeesBLLWithSprocs` clase y, desde las pestañas de seleccionar y eliminar, asegúrese de que el `GetEmployees` y `DeleteEmployee` métodos estén seleccionados en las listas desplegables. Haga clic en Finalizar para completar la configuración de s ObjectDataSource.


[![Configurar el ObjectDataSource para utilizar la clase EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figura 12**: configurar el ObjectDataSource que se utilizan los `EmployeesBLLWithSprocs` clase ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![Tiene el uso de ObjectDataSource las GetEmployees y los métodos de DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figura 13**: el uso de ObjectDataSource el `GetEmployees` y `DeleteEmployee` métodos ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio agregará un BoundField a GridView para cada uno de los `EmployeesDataTable` columnas s. Quite todos estos BoundFields excepto `Title`, `LastName`, `FirstName`, `ManagerFirstName`, y `ManagerLastName` y cambiar el nombre de la `HeaderText` propiedades para los cuatro últimos BoundFields apellido, nombre, nombre Manager s, y El Administrador de s apellidos, respectivamente.

Para permitir que los usuarios puedan eliminar a los empleados desde esta página que debemos hacer dos cosas. En primer lugar, indique a GridView para proporcionar capacidades de eliminación activando la opción Habilitar eliminación de la etiqueta inteligente. En segundo lugar, cambie la s ObjectDataSource `OldValuesParameterFormatString` propiedad desde el valor especificado mediante el Asistente de ObjectDataSource (`original_{0}`) en su valor predeterminado (`{0}`). Después de realizar estos cambios, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Probar la página visitando a través de un explorador. Como se muestra en la figura 14, mostrará la página de cada empleado y su nombre s manager (suponiendo que tienen uno).


[![La combinación en la Employees_Select procedimiento almacenado devuelve el nombre del administrador s](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figura 14**: el `JOIN` en el `Employees_Select` procedimiento almacenado devuelve el Administrador de s nombre ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Haga clic en el botón Eliminar inicia el flujo de trabajo eliminando, culmina en la ejecución de la `Employees_Delete` procedimiento almacenado. Sin embargo, el intento `DELETE` se produce un error en la instrucción en el procedimiento almacenado debido a una infracción de restricción de clave externa (vea la figura 15). En concreto, cada empleado tiene uno o más registros en la `Orders` tabla, que produce la eliminación de un error.


[![Eliminación de un empleado que tiene como consecuencia de pedidos correspondiente en una infracción de restricción de clave externa](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figura 15**: eliminación de un empleado que tiene como consecuencia de pedidos correspondiente en una infracción de restricción de clave externa ([haga clic aquí para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Para permitir a los empleados a ser eliminados se puede:

- Actualizar la restricción foreign key para la eliminación en cascada
- Elimine manualmente los registros de la `Orders` tabla para los empleados que desea eliminar, o
- Actualización de la `Employees_Delete` procedimiento almacenado para eliminar los registros relacionados de la `Orders` tabla antes de eliminar el `Employees` registro. Analizamos esta técnica en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial.

Dejar este como un ejercicio para el lector.

## <a name="summary"></a>Resumen

Cuando se trabaja con bases de datos relacionales, es común para las consultas extraer los datos de varias tablas relacionadas. Subconsultas correlacionadas y `JOIN` proporcionan dos técnicas distintas para tener acceso a datos de las tablas relacionadas en una consulta. En tutoriales anteriores hemos realizado suelen usan de subconsultas correlacionadas porque TableAdapter no puede Autogenerar `INSERT`, `UPDATE`, y `DELETE` instrucciones para las consultas que implican `JOIN` s. Mientras estos valores se pueden proporcionar manualmente, al usar instrucciones SQL ad hoc las personalizaciones se sobrescribirán cuando se completa el Asistente para configuración de TableAdapter.

Afortunadamente, los TableAdapters creados mediante los procedimientos almacenados no sufren la fragilidad mismo como los creados mediante instrucciones SQL ad hoc. Por lo tanto, resulta adecuado para crear un TableAdapter cuya consulta principal utiliza un `JOIN` cuando utilice procedimientos almacenados. En este tutorial, hemos visto cómo crear este tipo de un TableAdapter. Se inició mediante un `JOIN`-menos `SELECT` de consulta para la consulta principal de TableAdapter s para que la correspondiente insert, update y procedimientos almacenados de eliminación sería creado automáticamente. TableAdapter s inicial completado la configuración, hemos aumentado el `SelectCommand` procedimiento almacenado para usar un `JOIN` y vuelva a ejecutar el Asistente de configuración de TableAdapter para actualizar la `EmployeesDataTable` columnas s.

Volver a ejecutar el Asistente de configuración de TableAdapter se actualiza automáticamente la `EmployeesDataTable` columnas para reflejar los campos de datos devueltos por la `Employees_Select` procedimiento almacenado. Como alternativa, podríamos haber agregamos estas columnas manualmente a la tabla de datos. Exploraremos manualmente agrega columnas a la tabla de datos en el tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Hilton Geisenow, David Suru y Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Siguiente](adding-additional-datatable-columns-vb.md)
