---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: "Con las dependencias de caché SQL (C#) | Documentos de Microsoft"
author: rick-anderson
description: "La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expire después de un período de tiempo especificado. Pero este enfoque simple significa que los datos almacenados en caché maintai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: a6089b847dfd662e9b32128036170322823aac97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-sql-cache-dependencies-c"></a>Uso de las dependencias de caché SQL (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) o [descarga de PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expire después de un período de tiempo especificado. Pero este enfoque simple significa que los datos en caché no mantienen ninguna asociación con su origen de datos subyacente, lo que da lugar a datos obsoletos que es demasiado larga o los datos actuales que ha expirado demasiado pronto. Un mejor enfoque es utilizar la clase SqlCacheDependency para que los datos permanecen en caché hasta que se ha modificado los datos subyacentes en la base de datos SQL. Este tutorial muestra cómo.


## <a name="introduction"></a>Introducción

Examinan las técnicas de almacenamiento en caché en el [almacenar datos en caché con el ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) y [almacenar datos en caché en la arquitectura](caching-data-in-the-architecture-cs.md) tutoriales usan una expiración basada en el tiempo para expulsar los datos de la memoria caché después de un determinado período. Este enfoque es la manera más sencilla para equilibrar las mejoras de rendimiento del almacenamiento en caché contra la obsolescencia de datos. Si selecciona una expiración de tiempo de *x* segundos, un desarrollador de páginas concedes para disfrutar de las ventajas de rendimiento de almacenamiento en caché para solo *x* segundos, pero puede colocar la forma más fácil que sus datos nunca será obsoletos durante más de un máximo de *x* segundos. Por supuesto, para los datos estáticos, *x* se puede extender a la duración de la aplicación web, tal y como se examinó en el [almacenar datos en caché al iniciar la aplicación](caching-data-at-application-startup-cs.md) tutorial.

Al almacenar en caché datos de la base de datos, una expiración basada en el tiempo se elige a menudo por su facilidad de uso, pero suele ser una solución adecuada. Idealmente, los datos de la base de datos permanecerá en caché hasta que se ha modificado los datos subyacentes en la base de datos; solo entonces se desalojan la memoria caché. Este enfoque maximiza los beneficios de rendimiento de almacenamiento en caché y minimiza la duración de los datos obsoletos. Sin embargo, para poder disfrutar de estas ventajas se deben algún sistema en su lugar que conoce cuando los datos subyacentes de la base de datos se ha modificado y extrae los elementos correspondientes de la memoria caché. Antes de ASP.NET 2.0, los desarrolladores de páginas eran responsables de implementar este sistema.

ASP.NET 2.0 proporciona una [ `SqlCacheDependency` clase](https://msdn.microsoft.com/en-us/library/system.web.caching.sqlcachedependency.aspx) y la infraestructura necesaria para determinar cuándo se ha producido un cambio en la base de datos para que los correspondientes elementos almacenados en caché se pueda expulsar. Hay dos técnicas para determinar cuándo ha cambiado los datos subyacentes: sondeo y notificación. Después de tratar las diferencias entre la notificación y sondeo, vamos a crear la infraestructura necesaria para admitir el sondeo y, a continuación, explorar cómo usar el `SqlCacheDependency` escenarios clase declarativo y mediante programación.

## <a name="understanding-notification-and-polling"></a>Sondeo y la notificación de descripción

Hay dos técnicas que pueden utilizarse para determinar cuándo se ha modificado los datos en una base de datos: notificación y sondeo. Con la notificación, la base de datos de alertas automáticamente el tiempo de ejecución ASP.NET si los resultados de una consulta determinada se han cambiado desde que se ejecutó por última vez la consulta, en qué momento se expulsan los elementos almacenados en caché asociados a la consulta. Con el sondeo, el servidor de base de datos mantiene información sobre tablas han sido actualizó por última vez. El tiempo de ejecución ASP.NET sondea periódicamente la base de datos para comprobar las tablas que se han cambiado desde que se especificaron en la memoria caché. Las tablas cuyos datos se ha modificado tienen sus elementos de la memoria caché asociada expulsadas.

La opción de notificación requiere menos el programa de instalación de sondeo y es más específica, ya que realiza el seguimiento de cambios en el nivel de consulta, en lugar de en el nivel de tabla. Desafortunadamente, las notificaciones solo están disponibles en las ediciones completas de Microsoft SQL Server 2005 (es decir, las ediciones Express no). Sin embargo, la opción de sondeo puede utilizarse para todas las versiones de Microsoft SQL Server desde 7.0 para 2005. Puesto que estos tutoriales usan la edición Express de SQL Server 2005, nos centraremos en cómo configurar y usar la opción de sondeo. Consulte la sección aún más lecturas al final de este tutorial para obtener más recursos en las capacidades de notificación de SQL Server 2005 s.

Con el sondeo, la base de datos debe configurarse para incluir una tabla denominada `AspNet_SqlCacheTablesForChangeNotification` que tiene tres columnas: `tableName`, `notificationCreated`, y `changeId`. Esta tabla contiene una fila por cada tabla que contenga datos que necesitan para su uso en una dependencia de caché SQL en la aplicación web. El `tableName` columna especifica el nombre de la tabla mientras `notificationCreated` indica la fecha y hora que se agregó la fila a la tabla. El `changeId` columna es de tipo `int` y tiene un valor inicial de 0. Su valor se incrementa con cada modificación de la tabla.

Además el `AspNet_SqlCacheTablesForChangeNotification` tabla, la base de datos también debe incluir desencadenadores en cada una de las tablas que pueden aparecer en una dependencia de caché SQL. Estos desencadenadores se ejecutan cada vez que se inserta, actualiza o elimina una fila y aumentará la tabla s `changeId` valor en `AspNet_SqlCacheTablesForChangeNotification`.

El tiempo de ejecución ASP.NET realiza un seguimiento de la actual `changeId` para una tabla al almacenar en caché de datos mediante un `SqlCacheDependency` objeto. La base de datos se comprueba periódicamente y cualquier `SqlCacheDependency` objetos cuyo `changeId` difiere del valor en la base de datos se desalojan desde una diferencia `changeId` valor indica que ha habido un cambio en la tabla desde que se almacenó en caché los datos.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Paso 1: Explorar el`aspnet_regsql.exe`programa de línea de comandos

Con el enfoque de sondeo de la base de datos debe configurarse para que contenga la infraestructura se ha descrito anteriormente: tabla predefinida (`AspNet_SqlCacheTablesForChangeNotification`), una serie de procedimientos almacenados y desencadenadores en cada una de las tablas que se pueden usar en las dependencias de caché SQL en la web aplicación. Estas tablas, procedimientos almacenados y desencadenadores pueden crearse a través del programa de línea de comandos `aspnet_regsql.exe`, que se encuentra en la `$WINDOWS$\Microsoft.NET\Framework\version` carpeta. Para crear el `AspNet_SqlCacheTablesForChangeNotification` tabla y procedimientos almacenados asociados, ejecute lo siguiente desde la línea de comandos:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Para ejecutar estos comandos debe ser el inicio de sesión de base de datos especificada en el [ `db_securityadmin` ](https://msdn.microsoft.com/en-us/library/ms188685.aspx) y [ `db_ddladmin` ](https://msdn.microsoft.com/en-us/library/ms190667.aspx) roles. Para examinar el código T-SQL enviado a la base de datos por el `aspnet_regsql.exe` programa de línea de comandos, consulte [esta entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Por ejemplo, para agregar la infraestructura para el sondeo a una base de datos de Microsoft SQL Server denominada `pubs` en un servidor de base de datos denominado `ScottsServer` mediante la autenticación de Windows, navegue hasta el directorio adecuado y, desde la línea de comandos, escriba:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Una vez agregada la infraestructura de nivel de base de datos, es necesario agregar los desencadenadores a las tablas que se utilizarán en las dependencias de caché SQL. Usar el `aspnet_regsql.exe` línea de comandos del programa nuevo, pero especifique el nombre de tabla con el `-t` cambiar y, en lugar de usar el `-ed` cambiar uso `-et`, como:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Para agregar los desencadenadores para el `authors` y `titles` tablas en el `pubs` en la base de datos `ScottsServer`, use:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

Para este tutorial, agregue los desencadenadores para el `Products`, `Categories`, y `Suppliers` tablas. Echemos un vistazo la sintaxis de línea de comandos determinado en el paso 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Paso 2: Hacer referencia a una base de datos de Microsoft SQL Server 2005 Express Edition en`App_Data`

El `aspnet_regsql.exe` programa de línea de comandos requiere que el nombre de base de datos y el servidor con el fin de agregar la infraestructura de sondeo es necesario. Pero ¿cuál es el nombre de base de datos y del servidor para una base de datos Microsoft SQL Server 2005 Express que reside en el `App_Data` carpeta? En lugar de tener que detectar cuáles son los nombres de base de datos y el servidor, se ha encontrado que es el enfoque más sencillo adjuntar la base de datos a la `localhost\SQLExpress` instancia de base de datos y cambiar el nombre de los datos mediante [SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/ms174173.aspx). Si tiene una de las versiones completas de SQL Server 2005 instalados en su equipo, a continuación, es probable que ya tenga instalado en el equipo de SQL Server Management Studio. Si solo tiene la edición Express, puede descargar gratuitamente [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Empiece por cerrar Visual Studio. A continuación, abra SQL Server Management Studio y elija para conectarse a la `localhost\SQLExpress` server mediante la autenticación de Windows.


![Adjuntar a la localhost\SQLExpress Server](using-sql-cache-dependencies-cs/_static/image1.gif)

**Figura 1**: adjuntar a la `localhost\SQLExpress` Server


Después de conectarse al servidor, Management Studio se muestran el servidor y tener las subcarpetas de las bases de datos, seguridad y así sucesivamente. Haga doble clic en la carpeta de bases de datos y elija la opción de adjuntar. Se abrirá el cuadro de diálogo Adjuntar bases de datos cuadro (consulte la figura 2). Haga clic en el botón Agregar y seleccione el `NORTHWND.MDF` carpeta de base de datos en la s de la aplicación web `App_Data` carpeta.


[![Adjunte el archivo NORTHWND. Base de datos MDF desde la carpeta App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Figura 2**: adjuntar el `NORTHWND.MDF` base de datos de la `App_Data` carpeta ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image2.png))


Esto agregará la base de datos a la carpeta de bases de datos. El nombre de la base de datos podría ser la ruta de acceso completa al archivo de base de datos o la ruta de acceso completa se antepone con un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar tener que escribir en este nombre largo de la base de datos cuando se usa aspnet\_regsql.exe herramienta de línea de comandos, adjunta la base de datos a un nombre más fácil de usar con el botón secundario en la base de datos solo de cambio de nombre y elegir cambiar el nombre. Se ha cambiado el nombre mi base de datos a DataTutorials.


![Cambiar el nombre de la base de datos adjunto a un nombre más descriptivo del lenguaje](using-sql-cache-dependencies-cs/_static/image3.gif)

**Figura 3**: cambiar el nombre de la base de datos adjunto a un nombre más descriptivo del lenguaje


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Paso 3: Adición de la infraestructura de sondeo para la base de datos Northwind

Ahora que nos hemos conectado la `NORTHWND.MDF` base de datos de la `App_Data` carpeta, se re listo para agregar la infraestructura de sondeo. Suponiendo que se ha cambiado el nombre de la base de datos a DataTutorials, ejecute los cuatro comandos siguientes:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Después de ejecutar estos cuatro comandos, haga doble clic en el nombre de la base de datos en Management Studio, vaya al submenú tareas y elija Desasociar. A continuación, cierre Management Studio y vuelva a abrir Visual Studio.

Una vez que se vuelva a abrir Visual Studio, profundizar en la base de datos mediante el Explorador de servidores. Tenga en cuenta la nueva tabla (`AspNet_SqlCacheTablesForChangeNotification`), los nuevos procedimientos almacenan y los desencadenadores en el `Products`, `Categories`, y `Suppliers` tablas.


![La base de datos ahora incluye la infraestructura de sondeo es necesario](using-sql-cache-dependencies-cs/_static/image4.gif)

**Figura 4**: la base de datos ahora incluye la infraestructura de sondeo es necesario


## <a name="step-4-configuring-the-polling-service"></a>Paso 4: Configurar el servicio de sondeo

Después de crear las tablas necesarias, desencadenadores y procedimientos almacenados en la base de datos, el último paso es configurar el servicio de sondeo, que se realiza a través de `Web.config` mediante la especificación de las bases de datos de uso y la frecuencia de sondeo en milisegundos. El siguiente marcado sondea la base de datos de Northwind una vez por segundo.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

El `name` valor en el `<add>` elemento (NorthwindDB) asocia un nombre legible para el usuario a una base de datos determinada. Cuando se trabaja con las dependencias de caché SQL, necesitamos hacer referencia al nombre de la base de datos definido aquí, así como la tabla basada en los datos almacenados en caché. Veremos cómo usar la `SqlCacheDependency` clase para asociar mediante programación las dependencias de caché SQL con los datos en el paso 6 en caché.

Una vez que se ha establecido una dependencia de caché SQL, el sistema de sondeo se conectará a las bases de datos definidos en el `<databases>` elementos cada `pollTime` milisegundos y ejecutar el `AspNet_SqlCachePollingStoredProcedure` procedimiento almacenado. Este procedimiento almacenado - que se agregó de nuevo en el paso 3 usando la `aspnet_regsql.exe` herramienta de línea de comandos - devuelve el `tableName` y `changeId` valores para cada registro en `AspNet_SqlCacheTablesForChangeNotification`. Las dependencias de caché no actualizadas de SQL se desalojan de la memoria caché.

El `pollTime` configuración presenta un equilibrio entre rendimiento y la obsolescencia de datos. Una pequeña `pollTime` valor aumenta el número de solicitudes realizadas a la base de datos, pero más rápidamente extrae datos obsoletos de la memoria caché. Una mayor `pollTime` valor reduce el número de solicitudes de base de datos, pero aumenta el retardo entre cuando cambian los datos de back-end y cuando se expulsan los elementos de caché relacionados. Afortunadamente, la solicitud de la base de datos ejecuta un procedimiento almacenado simple s devolver unas pocas filas de una tabla simple y ligera. Pero experimentar con diferentes `pollTime` obsolescencia de acceso y los datos de la aplicación de base de datos de valores para encontrar un equilibrio entre el ideal. El más pequeño `pollTime` valor permitido es 500.

> [!NOTE]
> En el ejemplo anterior se proporciona una sola `pollTime` valor en el `<sqlCacheDependency>` elemento, pero puede especificar opcionalmente el `pollTime` valor en el `<add>` elemento. Esto es útil si tiene varias bases de datos especificadas y desea personalizar la frecuencia de sondeo por base de datos.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Paso 5: Mediante declaración trabajar con las dependencias de caché SQL

En los pasos 1 a 4 explicamos cómo configurar la infraestructura de la base de datos necesarios y configurar el sistema de sondeo. Con esta infraestructura en su lugar, ahora podemos agregar elementos a la caché de datos con una dependencia de caché SQL asociada con técnicas de programación o declarativas. En este paso, examinaremos cómo trabajar mediante declaración con las dependencias de caché SQL. En el paso 6 se examinará el enfoque mediante programación.

El [almacenar datos en caché con el ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial explora las capacidades de almacenamiento en caché declarativas de ObjectDataSource. Estableciendo simplemente la `EnableCaching` propiedad `true` y `CacheDuration` propiedad cierto intervalo de tiempo, ObjectDataSource almacenará automáticamente en memoria caché los datos devueltos por el objeto subyacente para el intervalo especificado. ObjectDataSource también puede utilizar una o más dependencias de caché SQL.

Para demostrar que con las dependencias de caché SQL mediante declaración, abra el `SqlCacheDependencies.aspx` página en el `Caching` carpeta y arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Establecer la s GridView `ID` a `ProductsDeclarative` y, en su etiqueta inteligente, elija para enlazar a un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative`.


[![Crear un nuevo origen ObjectDataSource denominado ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative` ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image4.png))


Configurar el ObjectDataSource para usar el `ProductsBLL` clase y establezca la lista desplegable en la ficha Seleccionar a `GetProducts()`. En la pestaña de la actualización, elija la `UpdateProduct` se puede sobrecargar con tres parámetros de entrada - `productName`, `unitPrice`, y `productID`. Establezca las listas desplegables para (ninguno) en las pestañas INSERT y DELETE.


[![Utilice la sobrecarga de UpdateProduct con tres parámetros de entrada](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Figura 6**: utilizar la sobrecarga de UpdateProduct con tres parámetros de entrada ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image6.png))


[![Establecer la lista desplegable en (ninguno) para la INSERCIÓN y eliminación pestañas](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Figura 7**: establecer la lista desplegable en (ninguno) para la INSERCIÓN y eliminar pestañas ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Quitar todos los campos, pero `ProductName`, `CategoryName`, y `UnitPrice`y dar formato a estos campos como considere oportuno. Desde la etiqueta inteligente de GridView s, active las casillas de verificación Habilitar paginación, habilitar la ordenación y Habilitar edición. Visual Studio establecerá la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`. En orden para la característica de edición de GridView s funcione correctamente, quite esta propiedad por completo de la sintaxis declarativa o el conjunto nuevo a su valor predeterminado, `{0}`.

Por último, agregue un control Label Web anteriormente la GridView y establezca su `ID` propiedad `ODSEvents` y su `EnableViewState` propiedad `false`. Después de realizar estos cambios, el marcado declarativo de s de página debe ser similar al siguiente. Tenga en cuenta que se ha realizado una serie de estéticas personalizaciones a los campos de GridView que no son necesarios para mostrar la funcionalidad de dependencia de caché SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el s ObjectDataSource `Selecting` eventos y en, agregue el código siguiente:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Recuerde que las operaciones de asignación ObjectDataSource `Selecting` solo cuando se recuperan datos de su objeto subyacente, se activa el evento. Si el ObjectDataSource accede a los datos desde su propia memoria caché, no se desencadena este evento.

Ahora, visite esta página a través de un explorador. Desde que se ve todavía para implementar el almacenamiento en caché, cada vez que la página, ordena o modificar la cuadrícula de la página debe mostrar el texto, si selecciona el evento, como se muestra en la figura 8.


[![El evento Selecting de ObjectDataSource s activa cada vez que se pagina el control GridView, editar, o el Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Figura 8**: The ObjectDataSource s `Selecting` evento se desencadena cada vez se pagina el control GridView, editada o Sorted ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image10.png))


Como vimos en el [almacenar datos en caché con el ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) tutorial, establecer el `EnableCaching` propiedad `true` hace que el ObjectDataSource para almacenar en caché los datos durante el tiempo especificado por su `CacheDuration` propiedad. ObjectDataSource también tiene un [ `SqlCacheDependency` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que agrega una o más dependencias de caché SQL a los datos almacenados en memoria caché usando el patrón:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Donde *databaseName* es el nombre de la base de datos como se especifica en el `name` atributo de la `<add>` elemento `Web.config`, y *tableName* es el nombre de la tabla de base de datos. Por ejemplo crear un ObjectDataSource que almacena en caché datos indefinidamente basándose en una dependencia de caché SQL con las operaciones de asignación Northwind `Products` de tabla, establezca la s ObjectDataSource `EnableCaching` propiedad `true` y su `SqlCacheDependency` propiedad NorthwindDB:Products.

> [!NOTE]
> Puede usar una dependencia de caché SQL *y* una expiración basado en tiempo estableciendo `EnableCaching` a `true`, `CacheDuration` para el intervalo de tiempo, y `SqlCacheDependency` a los nombres de la base de datos y tabla. ObjectDataSource expulsará sus datos cuando se alcanza el vencimiento basado en tiempo o cuando el sistema de sondeo detecta que la base de datos subyacente ha cambiado, lo que ocurra primero.


GridView en `SqlCacheDependencies.aspx` muestra los datos de dos tablas - `Products` y `Categories` (el producto s `CategoryName` campo se recupera a través de un `JOIN` en `Categories`). Por lo tanto, queremos especificar dos de las dependencias de caché SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurar el ObjectDataSource para admitir el almacenamiento en caché mediante las dependencias de caché SQL en productos y categorías](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Figura 9**: configurar el ObjectDataSource para compatibilidad con almacenamiento en caché utilizando SQL las dependencias de caché en `Products` y `Categories` ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image12.png))


Después de configurar el ObjectDataSource para admitir el almacenamiento en caché, volver a visitar la página a través de un explorador. De nuevo, el evento de selección de texto que se desencadena debe aparecer en la primera visita de página, pero debería desaparecer al paginar, ordenar o haga clic en los botones de edición o en Cancelar. Esto es porque después de cargan los datos en la caché de s ObjectDataSource, permanece allí hasta que el `Products` o `Categories` se modifican las tablas o los datos se actualizan a través de GridView.

Después de paginar a través de la cuadrícula y teniendo en cuenta la falta de los eventos de selección activa texto, abra una nueva ventana del explorador y navegue hasta el tutorial de conceptos básicos de la edición, inserción y eliminación de la sección (`~/EditInsertDelete/Basics.aspx`). Actualizar el nombre o el precio de un producto. A continuación, de a la primera ventana de explorador, ver una página de datos diferente, ordenar la cuadrícula o haga clic en un botón de edición de fila s. En esta ocasión, el evento de selección activada debe vuelve a aparecer, como la base de datos ha sido modificada (vea la figura 10). Si el texto no aparece, espere unos minutos e inténtelo de nuevo. Recuerde que el servicio de sondeo comprueba si hay cambios en el `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre cuando los datos subyacentes se actualizan y cuando se expulsan los datos en caché.


[![Modificación de la tabla Products extrae los datos del producto almacenado en caché](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Figura 10**: modificar la tabla de productos extrae los datos del producto almacenado en memoria caché ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Paso 6: Trabajar mediante programación con el`SqlCacheDependency`(clase)

El [almacenar datos en caché en la arquitectura](caching-data-in-the-architecture-cs.md) tutorial examinando las ventajas del uso de una capa de almacenamiento en caché independiente de la arquitectura en lugar de acoplando de manera precisa el almacenamiento en caché con el ObjectDataSource. En este tutorial, creamos un `ProductsCL` clase para mostrar trabajar mediante programación con la caché de datos. Para utilizar las dependencias de caché SQL en la capa de almacenamiento en caché, use la `SqlCacheDependency` clase.

Con el sistema de sondeo, un `SqlCacheDependency` objeto debe estar asociado con un par de base de datos y tabla determinado. El código siguiente, por ejemplo, se crea un `SqlCacheDependency` objeto basado en la base de datos de Northwind s `Products` tabla:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Los dos parámetros de entrada que el `SqlCacheDependency` constructor de s son los nombres de base de datos y tabla, respectivamente. Al igual que con la opción s ObjectDataSource `SqlCacheDependency` propiedad, el nombre de base de datos utilizado es el mismo que el valor especificado en el `name` atributo de la `<add>` elemento `Web.config`. El nombre de tabla es el nombre real de la tabla de base de datos.

Para asociar un `SqlCacheDependency` con un elemento agregado a la caché de datos, utilice uno de los `Insert` sobrecargas del método que acepta una dependencia. El código siguiente agrega *valor* a la caché de datos para una duración indefinida, pero lo asocia con un `SqlCacheDependency` en el `Products` tabla. En resumen, *valor* permanecerá en la memoria caché hasta que se desalojan debido a restricciones de memoria o porque el sistema de sondeo ha detectado que el `Products` tabla ha cambiado desde que se almacenó en caché.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Las capas de almacenamiento en caché de asignación `ProductsCL` clase actualmente almacena en caché los datos de la `Products` tabla con una expiración basado en tiempo de 60 segundos. Permiten s actualizar esta clase para que utilice en su lugar las dependencias de caché SQL. El `ProductsCL` clase s. `AddCacheItem` actualmente, método, que es responsable de agregar los datos a la memoria caché, contiene el código siguiente:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Actualizar este código para usar un `SqlCacheDependency` en lugar del objeto el `MasterCacheKeyArray` la dependencia de caché:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Para probar esta funcionalidad, agregue un control GridView a la página debajo existente `ProductsDeclarative` GridView. Establecer esta nueva s GridView `ID` a `ProductsProgrammatic` y, a través de la etiqueta inteligente, enlazarla a un nuevo ObjectDataSource denominado `ProductsDataSourceProgrammatic`. Configurar el ObjectDataSource para usar el `ProductsCL` (clase), establecer las listas desplegables en la instrucción SELECT y pestañas de actualización para `GetProducts` y `UpdateProduct`, respectivamente.


[![Configurar el ObjectDataSource para utilizar la clase ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Figura 11**: configurar el ObjectDataSource que se utilizan los `ProductsCL` clase ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image16.png))


[![Seleccione el método GetProducts en la lista de desplegable seleccione ficha s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Figura 12**: seleccione la `GetProducts` método desde la lista desplegable de ficha Seleccionar s ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image18.png))


[![Elija el método UpdateProduct de la lista desplegable de actualización pestaña s](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Figura 13**: elija el método UpdateProduct de la lista desplegable de ficha actualización s ([haga clic aquí para ver la imagen a tamaño completo](using-sql-cache-dependencies-cs/_static/image20.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Al igual que con el primer control GridView agregado a esta página, quite todos los campos, pero `ProductName`, `CategoryName`, y `UnitPrice`y dar formato a estos campos como considere oportuno. Desde la etiqueta inteligente de GridView s, active las casillas de verificación Habilitar paginación, habilitar la ordenación y Habilitar edición. Al igual que con la `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio se establecerá el `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` propiedad `original_{0}`. Para la característica de edición de GridView s funcionar correctamente, establezca esta propiedad nuevo a `{0}` (o quite por completo la asignación de propiedad de la sintaxis declarativa).

Después de completar estas tareas, el marcado declarativo GridView y ObjectDataSource resultante debe ser similar al siguiente:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Para probar el código SQL dependencia de la memoria caché en la capa de almacenamiento en caché de establece un punto de interrupción en la `ProductCL` clase s. `AddCacheItem` método y, a continuación, iniciar la depuración. Cuando visite `SqlCacheDependencies.aspx`, tal y como se solicitado por primera vez y se coloca en la memoria caché los datos, se alcanzará el punto de interrupción. A continuación, mover a otra página en el control GridView u ordenar una de las columnas. Esto hace que la GridView para volver a consultar sus datos, pero los datos deben encontrarse en la memoria caché desde el `Products` no se ha modificado la tabla de base de datos. Si los datos varias veces no se encuentran en la memoria caché, asegúrese de que hay suficiente memoria disponible en el equipo e inténtelo de nuevo.

Después de la paginación a través de algunas páginas de GridView, abra una segunda ventana del explorador y navegue hasta el tutorial de conceptos básicos de la edición, inserción y eliminación de la sección (`~/EditInsertDelete/Basics.aspx`). Actualizar un registro de la tabla Products y, a continuación, en la primera ventana de explorador, ver una página nueva o haga clic en uno de los encabezados de ordenación.

En este escenario verá uno de dos cosas: el punto de interrupción se tendrán en cuenta, que indica que los datos almacenados en caché se expulsan debido al cambio en la base de datos; o bien, el punto de interrupción no se tendrán en cuenta, lo que significa que `SqlCacheDependencies.aspx` se muestra ahora datos obsoletos. Si no se alcanza el punto de interrupción, es probable porque el servicio de sondeo aún no ha activado desde que se cambian los datos. Recuerde que el servicio de sondeo comprueba si hay cambios en el `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre cuando los datos subyacentes se actualizan y cuando se expulsan los datos en caché.

> [!NOTE]
> Este retraso es más probable que aparecen cuando se edita uno de los productos a través de GridView en `SqlCacheDependencies.aspx`. En el [almacenar datos en caché en la arquitectura](caching-data-in-the-architecture-cs.md) tutorial agregamos la `MasterCacheKeyArray` dependencia para asegurarse de que los datos que se está editando a través de la caché la `ProductsCL` clase s. `UpdateProduct` método fue expulsado de la memoria caché. Sin embargo, esta dependencia de la memoria caché se sustituye cuando se modifica la `AddCacheItem` método anteriormente en este paso y, por tanto, la `ProductsCL` clase seguirán mostrando los datos en caché hasta que el sistema de sondeo detecta el cambio a la `Products` tabla. Veremos cómo volver a introducir la `MasterCacheKeyArray` la dependencia en el paso 7 de caché.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Paso 7: Asociar varias dependencias de un elemento almacenado en caché

Recuerde que el `MasterCacheKeyArray` dependencia de memoria caché se utiliza para asegurarse de que *todos los* datos relacionados con el producto se expulsan de la memoria caché cuando se actualiza cualquier elemento único asociado dentro de él. Por ejemplo, el `GetProductsByCategoryID(categoryID)` método cachés `ProductsDataTables` instancias para cada único *categoryID* valor. Si uno de estos objetos se expulsa, la `MasterCacheKeyArray` dependencia de memoria caché se asegura de que los demás se quitan. Sin esta dependencia de la memoria caché, cuando se modifican los datos almacenados en memoria caché existe la posibilidad de que otros datos de productos almacenada en caché pueden quedar desactualizadas. Por lo tanto, lo importante que se mantiene la `MasterCacheKeyArray` dependencia de caché cuando se usa las dependencias de caché SQL. Sin embargo, la caché de datos s `Insert` método solo permite que un objeto de dependencia única.

Además, cuando se trabaja con las dependencias de caché SQL, podemos necesitamos asociar varias tablas de base de datos como dependencias. Por ejemplo, el `ProductsDataTable` en la caché en el `ProductsCL` clase contiene los nombres de categoría y el proveedor para cada producto, pero la `AddCacheItem` método sólo utiliza una dependencia en `Products`. En esta situación, si el usuario actualiza el nombre de una categoría o el proveedor, los datos almacenados en caché producto podrá permanecen en la caché y no está actualizado. Por lo tanto, queremos hacer que los datos de productos almacenada en caché dependa no solo la `Products` tabla, pero en el `Categories` y `Suppliers` tablas.

El [ `AggregateCacheDependency` clase](https://msdn.microsoft.com/en-us/library/system.web.caching.aggregatecachedependency.aspx) proporciona un medio para asociar varias dependencias con un elemento de caché. Comience creando un `AggregateCacheDependency` instancia. A continuación, agregue el conjunto de dependencias mediante la `AggregateCacheDependency` s `Add` método. Al insertar el elemento en la caché de datos a partir de ahí, pase el `AggregateCacheDependency` instancia. Cuando *cualquier* de la `AggregateCacheDependency` que cambian las dependencias de instancia s, el elemento almacenado en caché se expulsen.

La siguiente muestra el código actualizado para el `ProductsCL` clase s. `AddCacheItem` método. El método crea el `MasterCacheKeyArray` almacenar en caché dependencia junto con `SqlCacheDependency` de objetos para el `Products`, `Categories`, y `Suppliers` tablas. Éstos se combinan en una sola `AggregateCacheDependency` objeto denominado `aggregateDependencies`, que, a continuación, se pasa a la `Insert` método.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Pruebe este código nuevo. Ahora se cambia a la `Products`, `Categories`, o `Suppliers` tablas hacen que los datos almacenados en caché que se expulsen. Además, el `ProductsCL` clase s `UpdateProduct` expulsa del método, que se llama cuando se edita un producto a través de GridView, el `MasterCacheKeyArray` la dependencia, que hace que las almacenadas en caché de caché `ProductsDataTable` debe expulsarse y los datos que se van a volver a recuperar en el siguiente solicitud.

> [!NOTE]
> Las dependencias de caché SQL también pueden utilizarse con [caché de resultados](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para ver una demostración de esta funcionalidad, consulte: [Using ASP.NET Output Caching con SQL Server](https://msdn.microsoft.com/en-us/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Resumen

Al almacenar en caché datos de la base de datos, los datos lo ideal es que permanecerá en la memoria caché hasta que se han modificado en la base de datos. Con ASP.NET 2.0, las dependencias de caché SQL se pueden crear y usar en escenarios de declaración y mediante programación. Uno de los desafíos relacionados con este enfoque es para detectar si se ha modificado los datos. Las versiones completas de Microsoft SQL Server 2005 proporcionan capacidades de notificación que pueden emitir alertas una aplicación cuando ha cambiado un resultado de consulta. Para la Express Edition de SQL Server 2005 y versiones anteriores de SQL Server, un sistema de sondeo debe utilizarse en su lugar. Afortunadamente, la configuración de la infraestructura de sondeo es necesario es bastante sencilla.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Usar notificaciones de consulta en Microsoft SQL Server 2005](https://msdn.microsoft.com/en-us/library/ms175110.aspx)
- [Crear una notificación de consulta](https://msdn.microsoft.com/en-us/library/ms188669.aspx)
- [Almacenamiento en caché de ASP.NET con la `SqlCacheDependency` (clase)](https://msdn.microsoft.com/en-us/library/ms178604(VS.80).aspx)
- [Herramienta de registro de servidor de SQL de ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx)
- [Información general de`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Marko Rangel, Teresa Murphy y Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](caching-data-at-application-startup-cs.md)
[Siguiente](caching-data-with-the-objectdatasource-vb.md)
