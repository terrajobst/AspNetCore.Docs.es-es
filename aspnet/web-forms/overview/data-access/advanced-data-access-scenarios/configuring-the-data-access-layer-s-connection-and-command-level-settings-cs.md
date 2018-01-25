---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: "Configuración de opciones de nivel de conexión y comandos de la capa de acceso a datos (C#) | Documentos de Microsoft"
author: rick-anderson
description: "Los TableAdapters dentro de un conjunto de datos con tipo se ocupan automáticamente de conectarse a la base de datos, emitir comandos y rellenar un DataTable con los resultados..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: be81bde63d66c3a7070f31be830f7d10ba3a5f8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Configuración de opciones de nivel de conexión y comandos de la capa de acceso a datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) o [descarga de PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Los TableAdapters dentro de un conjunto de datos con tipo ocupan automáticamente de conectarse a la base de datos, emitir comandos y rellenar un DataTable con los resultados. Sin embargo, si desea ocuparse de estos detalles desarrollamos y, en este tutorial se obtenga información acerca de cómo obtener acceso a la configuración de nivel de conexión y comandos de base de datos de TableAdapter, hay ocasiones.


## <a name="introduction"></a>Introducción

A lo largo de la serie de tutoriales hemos utilizado los conjuntos de datos con tipo para implementar la capa de acceso a datos y objetos de negocios de nuestra arquitectura por niveles. Como se describe en el [primer tutorial](../introduction/creating-a-data-access-layer-cs.md), las DataTables actuar como repositorios de datos, mientras que los TableAdapters actúan como contenedores para comunicarse con la base de datos para recuperar y modificar los datos subyacentes de asignación de conjunto de datos con tipo. Los TableAdapters encapsulan la complejidad que implica trabajar con la base de datos y nos evita tener que escribir código para conectarse a la base de datos, emite un comando o rellenar los resultados en una tabla de datos.

Hay ocasiones, sin embargo, cuando necesitemos madriguera en las profundidades de TableAdapter y escribir código que funciona directamente con los objetos ADO.NET. En el [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial, por ejemplo, se agregan métodos al TableAdapter para principio, confirmar y revertir las transacciones de ADO.NET. Estos métodos utilizan un internos creados manualmente `SqlTransaction` objeto que se asignó a TableAdapters `SqlCommand` objetos.

En este tutorial, examinaremos cómo obtener acceso a la configuración de nivel de conexión y comandos de base de datos de TableAdapter. En concreto, vamos a agregar funcionalidad a la `ProductsTableAdapter` que permite el acceso a la cadena de conexión subyacente y configuración de tiempo de espera del comando.

## <a name="working-with-data-using-adonet"></a>Trabajar con datos mediante ADO.NET

Microsoft .NET Framework contiene una gran cantidad de clases diseñadas específicamente para trabajar con datos. Estas clases, se encuentra en la [ `System.Data` espacio de nombres](https://msdn.microsoft.com/library/system.data.aspx), se conocen como el *ADO.NET* clases. Algunas de las clases bajo el paraguas ADO.NET están asociados a un determinado *proveedor de datos*. Un proveedor de datos se puede considerar como un canal de comunicación que permite que la información fluya entre las clases ADO.NET y el almacén de datos subyacente. Hay proveedores generalizados, como OLE DB y ODBC, así como los proveedores que están especialmente diseñados para un sistema de base de datos determinada. Por ejemplo, aunque es posible conectarse a una base de datos de Microsoft SQL Server mediante el proveedor OleDb, el proveedor SqlClient es mucho más eficaz, tal como se ha diseñado y optimizado específicamente para SQL Server.

Al tener acceso a datos, normalmente se usa el patrón siguiente:

- Establecer una conexión con la base de datos.
- Emitir un comando.
- Para `SELECT` consultas, trabajar con los registros resultantes.

Hay diferentes clases ADO.NET para llevar a cabo cada uno de estos pasos. Para conectarse a una base de datos utiliza el proveedor SqlClient, por ejemplo, use la [ `SqlConnection` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Para emitir un `INSERT`, `UPDATE`, `DELETE`, o `SELECT` comando a la base de datos, use la [ `SqlCommand` clase](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Excepto para la [ajuste modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial, hemos no tuvimos que escribir cualquier código de ADO.NET de bajo nivel desarrollamos, ya que el código de los TableAdapters generada automáticamente incluye la funcionalidad necesaria para conectarse a la base de datos, ejecutar comandos, recuperar datos y rellenar los datos en tablas de datos. Sin embargo, puede haber ocasiones en que deba personalizar esta configuración de bajo nivel. En los pasos siguientes examinaremos cómo aprovechar los objetos ADO.NET que se usa internamente los TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Paso 1: Examinar con la propiedad de conexión

Cada clase de TableAdapter tiene un `Connection` propiedad que especifica la información de conexión de base de datos. Este tipo de datos de propiedad s y `ConnectionString` valor viene determinado por las selecciones realizadas en el Asistente para configuración de TableAdapter. Recuerde que cuando se agrega primero un TableAdapter a un DataSet con tipo este asistente pide us para la base de datos de origen (consulte la figura 1). La lista desplegable en este primer paso incluye las bases de datos especificados en el archivo de configuración, así como cualquier otras bases de datos en las conexiones de datos de asignación del explorador de servidores. Si la base de datos que deseamos utilizar no existe en la lista desplegable, puede especificarse una nueva conexión de base de datos haciendo clic en el botón nueva conexión y proporcionar la información de conexión necesaria.


[![El primer paso del Asistente para configuración de TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figura 1**: el primer paso del Asistente para configuración de TableAdapter ([haga clic aquí para ver la imagen a tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Permiten s Tómese un momento para inspeccionar el código para el s TableAdapter `Connection` propiedad. Como se indicó en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial, podemos ver el código de TableAdapter generado automáticamente en la ventana de vista de clases, profundizando hasta la clase adecuada y, a continuación, haga doble clic en el nombre del miembro.

Navegue a la ventana de vista de clases, vaya al menú Ver y elija Vista de clases (o escribiendo Ctrl + Mayús + C). En la mitad superior de la ventana de vista de clases, profundizar en los `NorthwindTableAdapters` espacio de nombres y seleccione la `ProductsTableAdapter` clase. Esto mostrará los `ProductsTableAdapter` miembros s en la parte inferior la mitad de la vista de clases, como se muestra en la figura 2. Haga doble clic en el `Connection` propiedad para ver su código.


![Haga doble clic en la propiedad de conexión en la vista de clases para ver el código generado automáticamente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figura 2**: haga doble clic en la propiedad de conexión en la vista de clases para ver el código generado automáticamente


Los TableAdapter s `Connection` propiedad y otras se indica a continuación código relacionado con la conexión:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Cuando se crea una instancia de la clase TableAdapter, la variable miembro `_connection` es igual a `null`. Cuando el `Connection` se accede a la propiedad, comprueba primero si el `_connection` se ha creado una instancia de la variable de miembro. Si no es así, tiene la `InitConnection` se invoca el método, que crea una instancia `_connection` y establece su `ConnectionString` propiedad en el valor de cadena de conexión especificado desde el primer de paso de s de Asistente para configuración de TableAdapter.

El `Connection` propiedad también se puede asignar a un `SqlConnection` objeto. Si lo hace, asocia el nuevo `SqlConnection` objeto a cada una de las operaciones de asignación TableAdapter `SqlCommand` objetos.

## <a name="step-2-exposing-connection-level-settings"></a>Paso 2: Exponer la configuración del nivel de conexión

La información de conexión debe permanecer encapsulada dentro de TableAdapter y no estar accesible para el resto de niveles en la arquitectura de la aplicación. Sin embargo, puede haber escenarios cuando la información de nivel de conexión de TableAdapter s tiene que ser accesible o puede personalizar para una consulta, un usuario o una página ASP.NET.

Permiten s ampliar la `ProductsTableAdapter` en el `Northwind` conjunto de datos para incluir un `ConnectionString` propiedad que puede utilizarse por la capa de lógica de negocios para leer o cambiar la cadena de conexión utilizada por el objeto TableAdapter.

> [!NOTE]
> A *cadena de conexión* es una cadena que especifica la información de conexión de base de datos, por ejemplo, el proveedor debe usar, la ubicación de la base de datos, las credenciales de autenticación y otra configuración relacionada con la base de datos. Para obtener una lista de patrones de cadena de conexión utilizado por una variedad de almacenes de datos y proveedores, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


Como se describe en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial, se pueden extender las clases de conjunto de datos con tipo s generado automáticamente mediante el uso de clases parciales. En primer lugar, cree una nueva subcarpeta en el proyecto denominado `ConnectionAndCommandSettings` debajo la `~/App_Code/DAL` carpeta.


![Agregar una subcarpeta denominada ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figura 3**: agregar una subcarpeta denominada`ConnectionAndCommandSettings`


Agregar un nuevo archivo de clase denominado `ProductsTableAdapter.ConnectionAndCommandSettings.cs` y escriba el código siguiente:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Esta clase parcial agrega un `public` propiedad denominada `ConnectionString` a la `ProductsTableAdapter` clase que permite que cualquier capa leer o actualizar la cadena de conexión para la s de TableAdapter conexión subyacente.

Con esta clase parcial crear (y guardar), abra la `ProductsBLL` clase. Vaya a uno de los métodos existentes y escriba en `Adapter` y, a continuación, presione la tecla perioda para que aparezca IntelliSense. Debería ver la nueva `ConnectionString` propiedad disponible en IntelliSense, lo que significa que puede leer o ajustar este valor de la capa BLL mediante programación.

## <a name="exposing-the-entire-connection-object"></a>Exponer el objeto de conexión

Esta clase parcial expone sólo una propiedad del objeto de conexión subyacente: `ConnectionString`. Si desea que el objeto de conexión disponible más allá de los límites del TableAdapter, o bien puede cambiar la `Connection` nivel de protección de la propiedad s. El código generado automáticamente, se examina en el paso 1 se ha explicado que los TableAdapter s `Connection` propiedad está marcada como `internal`, lo que significa que solo se accesible por clases en el mismo ensamblado. Esto se puede cambiar, sin embargo, a través de las operaciones de asignación TableAdapter `ConnectionModifier` propiedad.

Abra la `Northwind` conjunto de datos, haga clic en el `ProductsTableAdatper` en el diseñador y vaya a la ventana Propiedades. No existe, verá la `ConnectionModifier` establecido en su valor predeterminado, `Assembly`. Para realizar la `Connection` propiedad disponible fuera del ensamblado del conjunto de datos con tipo s, cambio el `ConnectionModifier` propiedad `Public`.


[![El nivel de accesibilidad de s de propiedad de conexión puede configurarse a través de la propiedad ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figura 4**: el `Connection` propiedad s accesibilidad nivel se pueden configurar a través de la `ConnectionModifier` propiedad ([haga clic aquí para ver la imagen a tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Guardar el conjunto de datos y, a continuación, volver a la `ProductsBLL` clase. Como antes, visite uno de los métodos existentes y escriba `Adapter` y, a continuación, presione la tecla perioda para que aparezca IntelliSense. La lista debe incluir un `Connection` propiedad, lo que significa que ahora puede mediante programación leer o asignar cualquier configuración de nivel de conexión de la capa BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Paso 3: Examinar las propiedades relacionadas con el comando

Un objeto TableAdapter consta de una consulta principal que, de forma predeterminada, se autogenerada `INSERT`, `UPDATE`, y `DELETE` las instrucciones. Esta consulta principal s `INSERT`, `UPDATE`, y `DELETE` instrucciones se implementan en el código de TableAdapter s como un objeto de adaptador de datos ADO.NET a través de la `Adapter` propiedad. Al igual que con su `Connection` propiedad, el `Adapter` tipo de datos de propiedad s está determinado por el proveedor de datos utilizado. Puesto que estos tutoriales usan el proveedor SqlClient, el `Adapter` propiedad es de tipo [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Los TableAdapter s `Adapter` propiedad tiene tres propiedades de tipo `SqlCommand` que usa para problema la `INSERT`, `UPDATE`, y `DELETE` instrucciones:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` objeto es responsable de enviar una consulta determinada en la base de datos y tiene propiedades, como: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), que contiene la instrucción de SQL ad hoc o procedimiento almacenado que se ejecuta; y [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), que es una colección de `SqlParameter` objetos. Como hemos visto en la [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) tutorial, estos comandos se pueden personalizar los objetos a través de la ventana Propiedades.

Además de su consulta principal, lo TableAdapter puede incluir un número variable de métodos que, cuando se invoca, envía un comando especificado para la base de datos. El objeto de comando de consulta principal s y los objetos de comando para todos los métodos adicionales se almacenan en las operaciones de asignación TableAdapter `CommandCollection` propiedad.

Permiten s Tómese un momento para examinar el código generado por el `ProductsTableAdapter` en el `Northwind` conjunto de datos para estas dos propiedades y sus variables miembro auxiliares y métodos auxiliares:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

El código para el `Adapter` y `CommandCollection` propiedades imita mejor que el de la `Connection` propiedad. Hay variables de miembro que contienen los objetos utilizados por las propiedades. Las propiedades `get` descriptores de acceso de inicio y comprueba si la variable de miembro correspondiente es `null`. Si es así, se llama a un método de inicialización que crea una instancia de la variable de miembro y asigna el núcleo de las propiedades relacionadas con el comando.

## <a name="step-4-exposing-command-level-settings"></a>Paso 4: Exponer la configuración del nivel de comando

Idealmente, la información del comando debe permanecer encapsulada dentro de la capa de acceso a datos. Debe ser necesita esta información en otras capas de la arquitectura, sin embargo, se puede exponer a través de una clase parcial, al igual que con la configuración de nivel de conexión.

Puesto que el TableAdapter solo tiene una sola `Connection` propiedad, el código para exponer la configuración del nivel de conexión es bastante sencillo. Las cosas son un poco más complicados cuando se modifica la configuración del nivel de comando porque el TableAdapter puede tener varios objetos de comando - un `InsertCommand`, `UpdateCommand`, y `DeleteCommand`, junto con un número variable de objetos de comando en el `CommandCollection` propiedad. Al actualizar la configuración del nivel de comando, esta configuración será necesario que se propaguen a todos los objetos de comando.

Por ejemplo, imagine que había algunas consultas de TableAdapter que tardó un extraordinario mucho tiempo en ejecutarse. Al utilizar el TableAdapter para ejecutar una de las consultas, que podría desear aumentar el objeto de comando s [ `CommandTimeout` propiedad](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Esta propiedad especifica el número de segundos que deben transcurrir para que se ejecute el comando y valor predeterminado es 30.

Para permitir la `CommandTimeout` propiedad que se va a ser ajustado por el BLL, agregue el siguiente `public` método a la `ProductsDataTable` usando el archivo de clase parcial creado en el paso 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Este método podría invocarse desde el BLL o capa de presentación para establecer el tiempo de espera de comando para todos los problemas de comandos por dicha instancia de TableAdapter.

> [!NOTE]
> El `Adapter` y `CommandCollection` propiedades están marcadas como `private`, lo que significa que solo sea accesible desde el código dentro del TableAdapter. A diferencia de la `Connection` propiedad, estos modificadores de acceso no son configurables. Por lo tanto, si es necesario exponer propiedades de nivel de comandos para otras capas de la arquitectura debe utilizar el método de clase parcial descrito anteriormente para proporcionar un `public` método o propiedad que se lee o escribe en el `private` objetos de comando.


## <a name="summary"></a>Resumen

Los TableAdapters dentro de un conjunto de datos con tipo sirven para encapsular los detalles de acceso a datos y la complejidad. Con los TableAdapters, no tenemos que preocuparse sobre cómo escribir código de ADO.NET para conectarse a la base de datos, emite un comando o rellenar los resultados en una tabla de datos. Se administra completamente automáticamente para que podamos.

Sin embargo, puede haber ocasiones cuando es necesario para personalizar las características ADO.NET bajo nivel, como cambiar la cadena de conexión o los valores de tiempo de espera de conexión o un comando de forma predeterminada. Lo TableAdapter se autogenerada `Connection`, `Adapter`, y `CommandCollection` propiedades, pero estos son `internal` o `private`, de forma predeterminada. Esta información interna puede exponerse mediante la ampliación del TableAdapter utilizando clases parciales para incluir `public` métodos o propiedades. Como alternativa, los TableAdapter s `Connection` modificador de acceso de propiedad se puede configurar a través de las operaciones de asignación TableAdapter `ConnectionModifier` propiedad.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Provocar a revisores para este tutorial se han Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy y Hilton Geisenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](working-with-computed-columns-cs.md)
[Siguiente](protecting-connection-strings-and-other-configuration-information-cs.md)
