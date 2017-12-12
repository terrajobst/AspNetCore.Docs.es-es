---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controles de origen de datos | Documentos de Microsoft
author: microsoft
description: "El control DataGrid de ASP.NET 1.x marcado una gran mejora en el acceso a datos en aplicaciones Web. Sin embargo, no es tan sencillo como podrían haber resultado..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: f40189796d3e25e9c337768cf04fdbfa293cdc2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="data-source-controls"></a>Controles de origen de datos
====================
por [Microsoft](https://github.com/microsoft)

> El control DataGrid de ASP.NET 1.x marcado una gran mejora en el acceso a datos en aplicaciones Web. Sin embargo, no como fácil de usar que podrían haber resultado. Aún se requería una cantidad considerable de código para obtener funcionalidad útil de él. Por ejemplo, es el modelo en todos los esfuerzos de acceso de datos en 1.x.


El control DataGrid de ASP.NET 1.x marcado una gran mejora en el acceso a datos en aplicaciones Web. Sin embargo, no como fácil de usar que podrían haber resultado. Aún se requería una cantidad considerable de código para obtener funcionalidad útil de él. Por ejemplo, es el modelo en todos los esfuerzos de acceso de datos en 1.x.

ASP.NET 2.0 soluciona este problema con parcialmente con controles de origen de datos. Los controles de origen de datos en ASP.NET 2.0 proporcionan a los desarrolladores un modelo declarativo para recuperar datos, mostrar los datos y editar datos. El propósito de los controles de origen de datos es proporcionar una representación coherente de datos a controles enlazados a datos independientemente del origen de esos datos. En el centro de los controles de origen de datos en ASP.NET 2.0 es la clase abstracta DataSourceControl. La clase DataSourceControl proporciona una implementación base de la interfaz IDataSource y la interfaz IListSource, el último de los cuales permite asignar el control de origen de datos como el origen de datos de un control enlazado a datos (a través de la nueva propiedad DataSourceId se explica más adelante) y exponer los datos en él como una lista. Cada lista de datos de un control de origen de datos se expone como un objeto DataSourceView. La interfaz IDataSource proporciona acceso a las instancias de DataSourceView. Por ejemplo, el método GetViewNames devuelve una ICollection que le permite enumerar los DataSourceViews asociado con un control de origen de datos determinado, y el método GetView permite obtener acceso a una instancia determinada de DataSourceView por su nombre.

Controles de origen de datos no tienen ninguna interfaz de usuario. Se implementan como controles de servidor para que pueda admitir la sintaxis declarativa y para que tengan acceso al estado de página si lo desea. Controles de origen de datos no representan ningún código HTML al cliente.

> [!NOTE]
> Como verá más adelante, son también caché beneficios obtenidos mediante el uso de controles de origen de datos.


## <a name="storing-connection-strings"></a>Almacenar cadenas de conexión

Antes de que accedemos a mirar cómo configurar los controles de origen de datos, debemos trataremos una nueva funcionalidad en ASP.NET 2.0 relativas a las cadenas de conexión. ASP.NET 2.0 presenta una nueva sección en el archivo de configuración que le permite almacenar fácilmente las cadenas de conexión que se pueden leer de forma dinámica en tiempo de ejecución. El &lt;connectionStrings&gt; sección facilita el proceso almacenar cadenas de conexión.

El fragmento de código siguiente agrega una nueva cadena de conexión.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Al igual que con la &lt;appSettings&gt; sección, el &lt;connectionStrings&gt; sección aparece fuera de la &lt;system.web&gt; sección en el archivo de configuración.


Para utilizar esta cadena de conexión, puede usar la sintaxis siguiente al establecer el atributo ConnectionString de un control de servidor.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

El &lt;connectionStrings&gt; sección también se puede cifrar para que no se exponga información confidencial. Esa capacidad se explicará en un módulo posterior.

## <a name="caching-data-sources"></a>Almacenamiento en caché de los orígenes de datos

Cada DataSourceControl proporciona cuatro propiedades para configurar el almacenamiento en caché; EnableCaching, CacheDuration, CacheExpirationPolicy y CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching es una propiedad booleana que determina si o no el almacenamiento en caché está habilitada para el control de origen de datos.

## <a name="cacheduration-property"></a>Propiedad CacheDuration

La propiedad CacheDuration establece el número de segundos que la memoria caché sigue siendo válida. Si se establece esta propiedad en **0** hace que siguen siendo válidas hasta que explícitamente no invalida la caché.

## <a name="cacheexpirationpolicy-property"></a>Propiedad CacheExpirationPolicy

La propiedad CacheExpirationPolicy puede establecerse en **absoluta** o **deslizante**. Si se establece en absoluto significa que la cantidad máxima de tiempo que los datos se almacenarán en caché es el número de segundos especificado por la propiedad CacheDuration. Si se establece en variable, la fecha de caducidad se restablece cuando se realiza cada operación.

## <a name="cachekeydependency-property"></a>Propiedad CacheKeyDependency

Si se especifica un valor de cadena para la propiedad CacheKeyDependency, ASP.NET configurará una nueva dependencia de caché basada en esa cadena. Esto le permite invalidar explícitamente la memoria caché simplemente cambiando o quitando el CacheKeyDependency.

**Importante**: si la suplantación está habilitada y el acceso al origen de datos o el contenido de los datos se basa en la identidad del cliente, se recomienda que se deshabilite el almacenamiento en caché estableciendo EnableCaching en False. Si está habilitada la caché en este escenario y un usuario distinto del usuario que originalmente solicitó los datos que emite una solicitud, no se exige la autorización para el origen de datos. Los datos simplemente se servirá de caché.

## <a name="the-sqldatasource-control"></a>El Control SqlDataSource

El control SqlDataSource permite que un desarrollador para acceder a los datos almacenados en cualquier base de datos relacional que es compatible con ADO.NET. Puede usar el proveedor System.Data.SqlClient para tener acceso a una base de datos de SQL Server, el proveedor System.Data.OleDb, el proveedor System.Data.Odbc o el proveedor de System.Data.OracleClient para tener acceso a Oracle. Por lo tanto, el SqlDataSource por supuesto, no sólo se utiliza para tener acceso a datos en una base de datos de SQL Server.

Para poder usar el SqlDataSource, simplemente proporciona un valor para la propiedad ConnectionString y especificar un comando SQL o procedimiento almacenado. El control SqlDataSource se encarga de trabajar con la arquitectura ADO.NET subyacente. Abre la conexión, consulta el origen de datos o ejecuta el procedimiento almacenado, devuelve los datos y, a continuación, cierra la conexión automáticamente.

> [!NOTE]
> Puesto que la clase DataSourceControl cierra automáticamente la conexión para usted, debe reducir el número de llamadas de cliente generado por la pérdida de conexiones de base de datos.


El siguiente fragmento de código enlaza un control DropDownList a un control SqlDataSource utilizando la cadena de conexión que se almacena en el archivo de configuración como se indicó anteriormente.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Tal y como se ilustra arriba, la propiedad DataSourceMode de SqlDataSource especifica el modo del origen de datos. En el ejemplo anterior, DataSourceMode se establece en DataReader. En ese caso, SqlDataSource devolverá un objeto de IDataReader mediante un cursor de solo avance y solo lectura. El tipo especificado del objeto que se devuelve se controla mediante el proveedor que se utiliza. En este caso, estoy usando el proveedor System.Data.SqlClient tal como se especifica en el &lt;connectionStrings&gt; sección del archivo web.config. Por lo tanto, el objeto devuelto será de tipo SqlDataReader. Al especificar un valor DataSourceMode del conjunto de datos, los datos pueden almacenarse en un conjunto de datos en el servidor. Este modo le permite agregar características, como la ordenación, paginación, etcetera. Si hubiese sido el enlace de datos SqlDataSource a un control GridView, habría elige el modo de conjunto de datos. Sin embargo, en el caso de DropDownList, el modo de DataReader es la opción correcta.

> [!NOTE]
> Al almacenar en caché un SqlDataSource o un AccessDataSource, debe establecerse la propiedad DataSourceMode al conjunto de datos. Si habilita el almacenamiento en caché con un DataSourceMode de DataReader, se producirá una excepción.


## <a name="sqldatasource-properties"></a>Propiedades de SqlDataSource

Las siguientes son algunas de las propiedades del control SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Un valor booleano que especifica si se cancela un comando select si uno de los parámetros es null. True de forma predeterminada.

### <a name="conflictdetection"></a>ConflictDetection

En una situación donde varios usuarios pueden actualizar un origen de datos al mismo tiempo, la propiedad ConflictDetection determina el comportamiento del control SqlDataSource. Esta propiedad se evalúa como uno de los valores de la enumeración ConflictOptions. Los valores son **CompareAllValues** y **OverwriteChanges**. Si se establece en OverwriteChanges, la última persona para escribir datos en el origen de datos sobrescribe todos los cambios anteriores. Sin embargo, si se establece la propiedad ConflictDetection en CompareAllValues, se crean parámetros para las columnas devueltas por SelectCommand y también se crean los parámetros para contener los valores originales en cada una de esas columnas que permiten la SqlDataSource para determinar si los valores han cambiado desde que se ejecutó el SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Establece u obtiene la cadena SQL utilizada al eliminar filas de la base de datos. Esto puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="deletecommandtype"></a>DeleteCommandType

Establece u obtiene al tipo de comando de eliminación, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Devuelve los parámetros utilizados por la DeleteCommand del objeto SqlDataSourceView asociado con el control SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Esta propiedad se utiliza para especificar el formato de los parámetros de valor original en casos donde se establece la propiedad ConflictDetection a CompareAllValues. El valor predeterminado es {0} lo que significa que los parámetros de valor original tendrá el mismo nombre que el parámetro original. En otras palabras, si el nombre del campo es Id. de empleado, el valor del parámetro original sería @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Establece u obtiene la cadena SQL que se utiliza para recuperar datos de la base de datos. Esto puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="selectcommandtype"></a>SelectCommandType

Establece u obtiene al tipo de comando select, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Devuelve los parámetros que se usan por SelectCommand del objeto SqlDataSourceView asociado con el control SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtiene o establece el nombre de un parámetro de procedimiento almacenado que se utiliza al ordenar los datos recuperados por el control de origen de datos. Sólo es válido cuando se establece SelectCommandType en StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Un punto y coma delimitada por la cadena que especifica las bases de datos y tablas utilizadas en una dependencia de caché de SQL Server. (Las dependencias de caché SQL se explicará en un módulo posterior.)

### <a name="updatecommand"></a>UpdateCommand

Establece u obtiene la cadena SQL que se utiliza al actualizar datos en la base de datos. Esto puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="updatecommandtype"></a>UpdateCommandType

Establece u obtiene al tipo de comando de actualización, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Devuelve los parámetros utilizados por el comando UpdateCommand del objeto SqlDataSourceView asociado con el control SqlDataSource.

## <a name="the-accessdatasource-control"></a>El Control AccessDataSource

El control AccessDataSource deriva de la clase SqlDataSource y se utiliza para enlazar datos a una base de datos de Microsoft Access. La propiedad ConnectionString para el control AccessDataSource es una propiedad de solo lectura. En lugar de utilizar la propiedad ConnectionString, se utiliza la propiedad de archivo de datos para que apunte a la base de datos de Access, tal y como se muestra a continuación.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

El AccessDataSource siempre establecerá ProviderName de SqlDataSource base en System.Data.OleDb y se conecta a la base de datos mediante el proveedor OLE DB Microsoft.Jet.OLEDB.4.0. No se puede utilizar el control AccessDataSource para conectarse a una base de datos de Access protegida con contraseña. Si tiene que conectarse a una base de datos protegida por contraseña, debe usar el control SqlDataSource.

> [!NOTE]
> Bases de datos de acceso almacenadas en el sitio Web deben colocarse en la aplicación\_directorio de datos. ASP.NET no admite archivos de este directorio que desea examinar. Debe conceder permisos de lectura y escritura a la aplicación de la cuenta de proceso\_directorio de datos al utilizar las bases de datos de Access.


## <a name="the-xmldatasource-control"></a>El Control XmlDataSource

El XmlDataSource se utiliza para enlazar datos XML a los controles enlazados a datos. Puede enlazar a un archivo XML utilizando la propiedad de archivo de datos o se puede enlazar a una cadena XML mediante la propiedad de datos. El XmlDataSource expone los atributos XML como campos enlazables. En casos donde es necesario para enlazar a valores que no se representan como atributos, debe usar una transformación XSL. También puede utilizar expresiones de XPath para filtrar los datos XML.

Tenga en cuenta el siguiente archivo XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Tenga en cuenta que el XmlDataSource utiliza una propiedad de XPath de *personas/persona* con el fin de filtrar solo los &lt;persona&gt; nodos. DropDownList, a continuación, enlaza datos con el atributo LastName mediante la propiedad DataTextField.

Mientras el control XmlDataSource se utiliza principalmente para enlazar a datos XML de solo lectura, es posible editar el archivo de datos XML. Tenga en cuenta que en estos casos, la inserción automática, actualización y eliminación de información en el archivo XML no se produzca automáticamente como ocurre con otros controles de origen de datos. En su lugar, tendrá que escribir código para editar manualmente los datos mediante los siguientes métodos del control XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera un objeto XmlDocument que contiene el código XML recuperado por el XmlDataSource.

### <a name="save"></a>Guardar

Guarda el objeto de XmlDocument en memoria en el origen de datos.

Es importante tener en cuenta que al método Save sólo funcionará cuando se cumplen las dos condiciones siguientes:

1. El XmlDataSource usa la propiedad de archivo de datos para enlazarse a un archivo XML en lugar de la propiedad de datos para enlazar a los datos XML en memoria.
2. Ninguna transformación se especifica mediante la propiedad TransformFile o transformación.

Tenga en cuenta también que el método Save puede producir resultados inesperados cuando se llama a varios usuarios simultáneamente.

## <a name="the-objectdatasource-control"></a>El Control ObjectDataSource

Los controles de origen de datos que hemos tratado hasta este punto son una elección excelente para las aplicaciones de dos niveles en los que el control de origen de datos se comunica directamente al almacén de datos. Sin embargo, muchas aplicaciones reales son aplicaciones de varios niveles en un control de origen de datos tal vez necesite para comunicarse con un objeto comercial que, a su vez, se comunica con la capa de datos. En estas situaciones, ObjectDataSource rellena la factura correctamente. ObjectDataSource funciona junto con un objeto de origen. El control ObjectDataSource creará una instancia del objeto de origen, llame al método especificado y dispose de la instancia de objeto todos dentro del ámbito de una solicitud única, si el objeto tiene métodos de instancia en lugar de métodos estáticos (Shared en Visual Basic). Por lo tanto, el objeto debe ser sin estado. Es decir, su objeto debe adquirir y liberar todos los recursos necesarios en un período de una única solicitud. Puede controlar cómo se crea el objeto de origen controlando el evento ObjectCreating del control ObjectDataSource. Puede crear una instancia del objeto de origen y, a continuación, establezca la propiedad ObjectInstance de la clase ObjectDataSourceEventArgs a dicha instancia. El control ObjectDataSource usará la instancia que se crea en el evento ObjectCreating en lugar de crear una instancia por sí mismo.

Si el objeto de origen para un control ObjectDataSource expone métodos estáticos públicos (Shared en Visual Basic) que se pueden llamar para recuperar y modificar datos, un control ObjectDataSource llamará a estos métodos directamente. Si un control ObjectDataSource debe crear una instancia del objeto de origen para realizar llamadas a métodos, el objeto debe incluir un constructor público que no toma ningún parámetro. El control ObjectDataSource llamará a este constructor cuando crea una nueva instancia del objeto de origen.

Si el objeto de origen no contiene un constructor público sin parámetros, puede crear una instancia del objeto de origen que se usará el control ObjectDataSource en el evento ObjectCreating.

## <a name="specifying-object-methods"></a>Especificar métodos de objeto

El objeto de origen para un control ObjectDataSource puede contener cualquier número de métodos que se utilizan para seleccionar, insertar, actualizar o eliminar datos. Estos métodos se invocan mediante el control ObjectDataSource basado en el nombre del método, según se identifica mediante el SelectMethod, InsertMethod, UpdateMethod o DeleteMethod propiedad del control ObjectDataSource. El objeto de origen puede incluir también un método SelectCount opcional, que se identifica mediante el control ObjectDataSource mediante la propiedad SelectCountMethod, que devuelve el recuento del número total de objetos en el origen de datos. El control ObjectDataSource llamará al método de SelectCount después de llamar a un método de selección para recuperar el número total de registros en el origen de datos para su uso al paginar.

## <a name="lab-using-data-source-controls"></a>Laboratorio mediante controles de origen de datos

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ejercicio 1: mostrar datos con el Control SqlDataSource

El siguiente ejercicio utiliza el control SqlDataSource para conectarse a la base de datos Northwind. Se supone que tiene acceso a la base de datos Northwind en una instancia de SQL Server 2000.

1. Cree un nuevo sitio web ASP.NET.
2. Agregue un nuevo archivo web.config.

    1. Haga doble clic en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
    2. Elija el archivo de configuración Web de la lista de plantillas y haga clic en Agregar.
3. Editar la &lt;connectionStrings&gt; sección como sigue: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Cambie a la vista de código y agregue un atributo ConnectionString y un atributo SelectCommand a la &lt;asp: SqlDataSource&gt; controlar como se indica a continuación: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Desde la vista Diseño, agregue un nuevo control GridView.
6. En la lista desplegable de Elegir origen de datos en el menú tareas de GridView, elija SqlDataSource1.
7. Haga doble clic en Default.aspx y elija ver en el explorador en el menú. Cuando se le pida para guardar, haga clic en Sí.
8. GridView muestra los datos de la tabla Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ejercicio 2: modificar datos con el Control SqlDataSource

El siguiente ejercicio muestra cómo enlazar los datos DropDownList controlar mediante la sintaxis declarativa y permite editar los datos presentados en el control DropDownList.

1. En la vista de diseño, retire el control GridView Default.aspx. 

    **Importante**: deje el control SqlDataSource en la página.
2. Agregar un control DropDownList a Default.aspx.
3. Cambie a la vista de origen.
4. Agregue un atributo DataSourceId, DataTextField y DataValueField a la &lt;asp: DropDownList&gt; controlar como se indica a continuación: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Guarde Default.aspx y verlo en el explorador. Tenga en cuenta que la DropDownList contiene todos los productos de la base de datos Northwind.
6. Cierre el explorador.
7. En la vista de código fuente de Default.aspx, agregue un nuevo control de cuadro de texto debajo del control DropDownList. Cambie la propiedad de Id. del cuadro de texto a txtProductName.
8. Bajo el control de cuadro de texto, agregue un nuevo control de botón. Cambie la propiedad ID del botón btnUpdate y la propiedad Text a **Update Product Name**.
9. En la vista de código fuente de Default.aspx, agregue una propiedad UpdateCommand y dos UpdateParameters nueva a la etiqueta de SqlDataSource como sigue: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Tenga en cuenta que hay que dos parámetros (ProductName y ProductID) que se agregan en este código de actualización. Estos parámetros se asignan a la propiedad de texto de la txtProductName cuadro de texto y la propiedad SelectedValue de la ddlProducts DropDownList.
10. Cambie a la vista Diseño y haga doble clic en el control de botón para agregar un controlador de eventos.
11. Agregue el código siguiente a la btnUpdate\_haga clic en el código: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Haga doble clic en Default.aspx y elija ver en el explorador. Cuando se le pida para guardar todos los cambios, haga clic en Sí.
13. ASP.NET 2.0 permiten clases parciales para la compilación en tiempo de ejecución. No es necesario compilar una aplicación con el fin de ver los cambios de código surta efecto.
14. Seleccionar un producto de la lista desplegable.
15. Escriba un nombre nuevo para el producto seleccionado en el cuadro de texto y, a continuación, haga clic en el botón de actualización.
16. El nombre del producto se actualiza en la base de datos.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ejercicio 3 mediante el Control ObjectDataSource

Este ejercicio muestra cómo usar el control ObjectDataSource y un objeto de origen para interactuar con la base de datos Northwind.

1. Haga doble clic en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
2. Seleccione el formulario Web Forms en la lista de plantillas. Cambie el nombre a object.aspx y haga clic en Agregar.
3. Haga doble clic en el proyecto en el Explorador de soluciones y haga clic en Agregar nuevo elemento.
4. Seleccione la clase en la lista de plantillas. Cambie el nombre de la clase a NorthwindData.cs y haga clic en Agregar.
5. Haga clic en Sí cuando se le solicite para agregar la clase a la aplicación\_carpeta de código.
6. Agregue el código siguiente al archivo NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Agregue el código siguiente a la vista del origen de object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Guarde todos los archivos y examinar object.aspx.
9. Interactuar con la interfaz de ver los detalles, edición de los empleados, agregando a los empleados, y eliminando a los empleados.
