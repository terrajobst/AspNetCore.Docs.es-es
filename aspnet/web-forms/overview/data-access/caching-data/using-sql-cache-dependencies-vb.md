---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Usar dependencias de caché SQL (VB) | Microsoft Docs
author: rick-anderson
description: La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expire después de un período de tiempo especificado. Pero este enfoque simple significa que los datos almacenados en caché de maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: f47cc7c1fd4fd0d1e41bef31a2e68dd34393d52e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838612"
---
<a name="using-sql-cache-dependencies-vb"></a>Usar dependencias de caché SQL (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) o [descargar PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expire después de un período de tiempo especificado. Pero este enfoque simple significa que los datos en caché no mantienen ninguna asociación con su origen de datos subyacente, lo que produce datos obsoletos que se mantengan demasiado tiempo o los datos actuales que haya expirado demasiado pronto. Un mejor enfoque es usar la clase SqlCacheDependency para que los datos permanecen en caché hasta que se ha modificado los datos subyacentes en la base de datos SQL. Este tutorial le enseña cómo hacerlo.


## <a name="introduction"></a>Introducción

Examinan las técnicas de almacenamiento en caché en el [datos de almacenamiento en caché con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) y [almacenamiento en caché de datos en la arquitectura](caching-data-in-the-architecture-vb.md) tutoriales usan una expiración de duración definida para expulsar los datos de la memoria caché después de un determinado período. Este enfoque es la manera más sencilla para equilibrar las mejoras de rendimiento de almacenamiento en caché contra la obsolescencia de datos. Si selecciona una caducidad de tiempo de *x* segundos, un desarrollador de páginas concedes para disfrutar de las ventajas de rendimiento de almacenamiento en caché de solo *x* segundos, pero puede estar tranquilo que sus datos nunca será obsoletos durante más de un máximo de *x* segundos. Por supuesto, para los datos estáticos, *x* se puede extender a la duración de la aplicación web, según se examinan en el [datos de almacenamiento en caché al iniciarse la aplicación](caching-data-at-application-startup-vb.md) tutorial.

Al almacenar en caché de la base de datos, una expiración de duración definida se elige a menudo por su facilidad de uso, pero con frecuencia es una solución adecuada. Idealmente, la base de datos podría permanecer en caché hasta que se ha modificado los datos subyacentes en la base de datos; solo, a continuación, podría expulsada la memoria caché. Este enfoque maximiza las ventajas de rendimiento de almacenamiento en caché y minimiza la duración de los datos obsoletos. Sin embargo, para disfrutar de estas ventajas se debe algún sistema en su lugar que reconoce cuando la base de datos subyacente se ha modificado y extrae los elementos correspondientes de la memoria caché. Antes de ASP.NET 2.0, los desarrolladores de páginas eran responsables de implementar este sistema.

ASP.NET 2.0 proporciona una [ `SqlCacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) y la infraestructura necesaria para determinar cuándo ha producido un cambio en la base de datos para que los correspondientes elementos almacenados en caché que se pueda expulsar. Hay dos técnicas para determinar cuándo ha cambiado los datos subyacentes: sondeo y notificación. Después de explicar las diferencias entre la notificación y sondeo, vamos a crear la infraestructura necesaria para admitir el sondeo y, a continuación, exploramos cómo usar el `SqlCacheDependency` escenarios clase declarativo y mediante programación.

## <a name="understanding-notification-and-polling"></a>Sondeo y la notificación de descripción

Hay dos técnicas que pueden usarse para determinar cuándo se ha modificado los datos en una base de datos: notificación y el sondeo. Con la notificación, la base de datos de alertas automáticamente el tiempo de ejecución ASP.NET cuando los resultados de una consulta determinada se han cambiado desde que se ejecutó por última vez la consulta, momento en que se expulsan los elementos almacenados en caché asociados con la consulta. Con el sondeo, el servidor de base de datos mantiene información sobre cuándo se actualizaron por última vez tablas. El tiempo de ejecución ASP.NET sondea periódicamente la base de datos para comprobar las tablas han cambiado desde que se especificaron en la memoria caché. Las tablas cuyos datos se ha modificado tienen sus elementos de caché asociada que se elimina.

La opción de notificación requiere una configuración menos que el sondeo y es más granular, ya que realiza un seguimiento de cambios en el nivel de consulta en lugar de en el nivel de tabla. Lamentablemente, las notificaciones solo están disponibles en las ediciones completas de Microsoft SQL Server 2005 (es decir, las ediciones que no son Express). Sin embargo, se puede usar la opción de sondeo para todas las versiones de Microsoft SQL Server de 7.0 a 2005. Dado que estos tutoriales usan la edición Express de SQL Server 2005, nos centraremos en cómo configurar y usar la opción de sondeo. Consulte la sección Lecturas aún más al final de este tutorial aún más recursos sobre las capacidades de notificación de SQL Server 2005 s.

Con el sondeo, la base de datos debe configurarse para incluir una tabla denominada `AspNet_SqlCacheTablesForChangeNotification` que tiene tres columnas: `tableName`, `notificationCreated`, y `changeId`. Esta tabla contiene una fila por cada tabla que contenga datos que necesitan para su uso en una dependencia de caché SQL en la aplicación web. El `tableName` columna especifica el nombre de la tabla mientras `notificationCreated` indica la fecha y hora que se agregó la fila a la tabla. El `changeId` columna es de tipo `int` y tiene un valor inicial de 0. Su valor se incrementa con cada modificación de la tabla.

Además el `AspNet_SqlCacheTablesForChangeNotification` tabla, la base de datos debe incluir también los desencadenadores en cada una de las tablas que pueden aparecer en una dependencia de caché SQL. Estos desencadenadores se ejecutan cada vez que se inserta, actualiza o elimina una fila y aumentará la tabla s `changeId` valor en `AspNet_SqlCacheTablesForChangeNotification`.

Realiza el seguimiento de la ejecución de ASP.NET actual `changeId` para una tabla al almacenar en caché de datos mediante un `SqlCacheDependency` objeto. Se comprueba periódicamente la base de datos y cualquier `SqlCacheDependency` objetos cuya propiedad `changeId` difiere del valor en la base de datos se expulsan desde una diferencia `changeId` valor indica que ha habido un cambio en la tabla desde que se almacenó en caché los datos.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Paso 1: Explorar el`aspnet_regsql.exe`programa de línea de comandos

Con el enfoque de sondeo de la base de datos debe configurarse para contener la infraestructura se ha descrito anteriormente: una tabla predefinida (`AspNet_SqlCacheTablesForChangeNotification`), una serie de procedimientos almacenados y desencadenadores en cada una de las tablas que se pueden usar en las dependencias de caché SQL en la web aplicación. Estas tablas, procedimientos almacenados y desencadenadores pueden crearse a través del programa de línea de comandos `aspnet_regsql.exe`, que se encuentra en la `$WINDOWS$\Microsoft.NET\Framework\version` carpeta. Para crear el `AspNet_SqlCacheTablesForChangeNotification` tabla y procedimientos almacenados asociados, ejecute lo siguiente desde la línea de comandos:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Para ejecutar estos comandos debe ser el inicio de sesión de base de datos especificada en el [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) y [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) roles. Para examinar la envía a la base de datos mediante T-SQL el `aspnet_regsql.exe` programa de línea de comandos, consulte [esta entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Por ejemplo, para agregar la infraestructura para el sondeo a una base de datos de Microsoft SQL Server denominada `pubs` en un servidor de base de datos denominado `ScottsServer` mediante la autenticación de Windows, navegue hasta el directorio adecuado y, desde la línea de comandos, escriba:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Una vez agregada la infraestructura de nivel de base de datos, es necesario agregar los desencadenadores a las tablas que se usará en las dependencias de caché SQL. Utilice la `aspnet_regsql.exe` línea de comandos del programa nuevo, pero especifique el nombre de tabla con el `-t` cambiar y, en lugar de usar el `-ed` cambiar uso `-et`, así:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Para agregar los desencadenadores para el `authors` y `titles` tablas en el `pubs` en la base de datos `ScottsServer`, use:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

En este tutorial, agregue los desencadenadores para el `Products`, `Categories`, y `Suppliers` tablas. Examinaremos la sintaxis de línea de comandos determinada en el paso 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Paso 2: Hacer referencia a una base de datos de Microsoft SQL Server 2005 Express Edition en`App_Data`

El `aspnet_regsql.exe` programa de línea de comandos requiere el nombre del servidor y base de datos con el fin de agregar la infraestructura de sondeo es necesario. ¿Pero cuál es el nombre de base de datos y servidor para una base de datos Microsoft SQL Server 2005 Express que se encuentra en la `App_Data` carpeta? En lugar de tener que detectar cuáles son los nombres de base de datos y servidor, se ve encuentra que es el enfoque más sencillo adjuntar la base de datos para el `localhost\SQLExpress` instancia de base de datos y cambiar el nombre de los datos mediante [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Si tiene una de las versiones completas de SQL Server 2005 instalado en el equipo, a continuación, es probable que ya tenga instalado en el equipo de SQL Server Management Studio. Si solo tiene la edición Express, puede descargar la versión gratuita [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Empiece por cerrar Visual Studio. A continuación, abra SQL Server Management Studio y elija para conectarse a la `localhost\SQLExpress` server mediante la autenticación de Windows.


![Adjuntar a la localhost\SQLExpress Server](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figura 1**: adjuntar a la `localhost\SQLExpress` Server


Después de conectarse al servidor, Management Studio mostrará el servidor y tener subcarpetas para las bases de datos, seguridad y así sucesivamente. Haga doble clic en la carpeta de bases de datos y elija la opción para adjuntar. Se abrirá el cuadro de diálogo Adjuntar bases de datos cuadro (consulte la figura 2). Haga clic en el botón Agregar y seleccione el `NORTHWND.MDF` carpeta de base de datos en aplicaciones web s `App_Data` carpeta.


[![Adjunte el archivo NORTHWND. Base de datos MDF desde la carpeta App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figura 2**: adjuntar el `NORTHWND.MDF` desde la base de datos la `App_Data` carpeta ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image2.png))


Esto agregará la base de datos a la carpeta de bases de datos. El nombre de la base de datos podría ser la ruta de acceso completa al archivo de base de datos o la ruta de acceso completa se antepone un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar tener que escribir en este nombre largo de la base de datos cuando se usa aspnet\_regsql.exe herramienta de línea de comandos, adjunta la base de datos a un nombre más fácil de usar con el botón secundario en la base de datos solo de cambio de nombre y elegir cambiar el nombre. Se ve cambiado mi base de datos a DataTutorials.


![Cambiar el nombre de la base de datos adjunta a un nombre más fácil de usar](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figura 3**: cambiar el nombre de la base de datos adjunta a un nombre más fácil de usar


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Paso 3: Adición de la infraestructura de sondeo a la base de datos Northwind

Ahora que nos hemos conectado la `NORTHWND.MDF` desde la base de datos la `App_Data` carpeta, que está listo para agregar la infraestructura de sondeo. Suponiendo que ha cambiado el nombre de la base de datos a DataTutorials, ejecute los siguientes cuatro comandos:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Después de ejecutar estos cuatro comandos, haga doble clic en el nombre de la base de datos en Management Studio, vaya al submenú tareas y elija Desasociar. A continuación, cierre Management Studio y vuelva a abrir Visual Studio.

Una vez que se vuelve a abrir Visual Studio, profundizar en la base de datos mediante el Explorador de servidores. Tenga en cuenta la nueva tabla (`AspNet_SqlCacheTablesForChangeNotification`), los nuevos procedimientos almacenan y los desencadenadores en el `Products`, `Categories`, y `Suppliers` tablas.


![La base de datos ahora incluye la infraestructura necesaria de sondeo](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figura 4**: la base de datos ahora incluye la infraestructura necesaria de sondeo


## <a name="step-4-configuring-the-polling-service"></a>Paso 4: Configurar el servicio de sondeo

Después de crear las tablas necesarias, desencadenadores y procedimientos almacenados en la base de datos, el último paso es configurar el servicio de sondeo, que se realiza a través `Web.config` mediante la especificación de las bases de datos que se usará y la frecuencia de sondeo en milisegundos. El siguiente marcado sondea la base de datos de Northwind una vez por segundo.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

El `name` valor en el `<add>` elemento (NorthwindDB) asocia un nombre legible con una base de datos determinada. Al trabajar con dependencias de caché de SQL, necesitamos hacer referencia al nombre de la base de datos definido aquí, así como la tabla basada en los datos en caché. Veremos cómo usar el `SqlCacheDependency` clase para asociar mediante programación las dependencias de caché SQL con los datos en el paso 6 en caché.

Una vez que se ha establecido una dependencia de caché SQL, el sistema de sondeo se conectará a las bases de datos definidos en el `<databases>` elementos cada `pollTime` milisegundos y ejecute el `AspNet_SqlCachePollingStoredProcedure` procedimiento almacenado. Realizar una copia de este procedimiento almacenado - que se agregó en el paso 3 usando los `aspnet_regsql.exe` herramienta de línea de comandos - devuelve el `tableName` y `changeId` valores para cada registro en `AspNet_SqlCacheTablesForChangeNotification`. Dependencias de caché obsoletas de SQL se expulsan de la memoria caché.

El `pollTime` configuración presenta un equilibrio entre rendimiento y la obsolescencia de datos. Una pequeña `pollTime` valor aumenta el número de solicitudes a la base de datos, pero más rápidamente extrae datos obsoletos de la memoria caché. Una mayor `pollTime` valor reduce el número de solicitudes de base de datos, pero aumenta el retraso entre cuando cambian los datos de back-end y cuando se expulsan los elementos relacionados de caché. Afortunadamente, la solicitud de la base de datos ejecuta un procedimiento almacenado simple s devolver unas pocas filas de una tabla simple y ligera. Pero experimentar con diferentes `pollTime` valores para encontrar un equilibrio ideal entre obsolescencia de datos y acceso para la aplicación de base de datos. El más pequeño `pollTime` valor permitido es de 500.

> [!NOTE]
> En el ejemplo anterior se proporciona un único `pollTime` valor en el `<sqlCacheDependency>` elemento, pero puede especificar opcionalmente el `pollTime` valor en el `<add>` elemento. Esto es útil si tiene varias bases de datos especificados y desea personalizar la frecuencia de sondeo por base de datos.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Paso 5: Mediante declaración trabajar con dependencias de caché de SQL

En los pasos 1 a 4 vimos cómo configurar la infraestructura de la base de datos necesarios y configurar el sistema de sondeo. Con esta infraestructura en su lugar, ahora podemos agregar elementos a la caché de datos con una dependencia de caché asociada de SQL mediante técnicas declarativas o mediante programación. En este paso, examinaremos cómo trabajar de forma declarativa con dependencias de caché de SQL. En el paso 6 buscaremos el enfoque mediante programación.

El [datos de almacenamiento en caché con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) tutorial explora las capacidades de almacenamiento en caché declarativas del origen ObjectDataSource. Simplemente estableciendo el `EnableCaching` propiedad `True` y el `CacheDuration` propiedad cierto intervalo de tiempo, el origen ObjectDataSource almacenará automáticamente en caché los datos devueltos desde su objeto base para el intervalo especificado. ObjectDataSource también puede usar una o varias dependencias de caché SQL.

Para demostrar el uso de forma declarativa las dependencias de caché SQL, abra el `SqlCacheDependencies.aspx` página en el `Caching` carpetas y arrastre un control GridView del cuadro de herramientas hasta el diseñador. Establezca la s GridView `ID` a `ProductsDeclarative` y, en su etiqueta inteligente, elija para enlazarla a un nuevo origen ObjectDataSource denominado `ProductsDataSourceDeclarative`.


[![Crear un nuevo origen ObjectDataSource denominado ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figura 5**: crear un nuevo origen ObjectDataSource denominado `ProductsDataSourceDeclarative` ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image4.png))


Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase y establezca la lista desplegable en la ficha para seleccionar `GetProducts()`. En la pestaña de la actualización, elija el `UpdateProduct` sobrecargar con tres parámetros de entrada - `productName`, `unitPrice`, y `productID`. Establecer las listas desplegables en (None) en las pestañas INSERT y DELETE.


[![Utilice la sobrecarga de UpdateProduct con tres parámetros de entrada](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figura 6**: utilizar la sobrecarga de UpdateProduct con tres parámetros de entrada ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image6.png))


[![Establecer la lista desplegable en (None) para las fichas DELETE y INSERT](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figura 7**: establecer la lista desplegable en (None) para la INSERCIÓN y eliminar pestañas ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Quitar todos los campos, pero `ProductName`, `CategoryName`, y `UnitPrice`y dar formato a estos campos como considere oportuno. En la etiqueta inteligente s GridView, active las casillas Enable Paging, Enable Sorting y Habilitar edición. Visual Studio establecerá la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`. En el orden de la característica de edición de GridView s funcione correctamente, quite esta propiedad completamente de la sintaxis declarativa o volver a establecer en su valor predeterminado, `{0}`.

Por último, agregue un control Label Web anteriormente la GridView y establezca su `ID` propiedad `ODSEvents` y su `EnableViewState` propiedad `False`. Después de realizar estos cambios, el marcado declarativo s de página debe ser similar al siguiente. Tenga en cuenta que he llegado una serie de estéticas personalizaciones a los campos de GridView que no son necesarios para demostrar la funcionalidad de dependencia de caché SQL.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el s ObjectDataSource `Selecting` eventos y en agregar el código siguiente:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Recuerde que las operaciones de asignación ObjectDataSource `Selecting` solo cuando se recuperan datos de su objeto subyacente, se activa el evento. Si el origen ObjectDataSource tiene acceso a los datos de su propia memoria caché, no se desencadena este evento.

Ahora, visite esta página a través de un explorador. Desde que creamos ve todavía para implementar cualquier almacenamiento en caché, cada vez que la página, ordena o modificar la cuadrícula de la página debe mostrar el texto, cuando se selecciona el evento, como se muestra en la figura 8.


[![El evento cuando se selecciona s de ObjectDataSource activa cada vez que se pagina el control GridView, editar, o el ordenado](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figura 8**: The ObjectDataSource s `Selecting` Sorted, editados o evento se desencadena cada vez se pagina el control GridView ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image10.png))


Como hemos visto en el [datos de almacenamiento en caché con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) tutorial, establecer el `EnableCaching` propiedad `True` hace que el ObjectDataSource para almacenar en caché sus datos durante el tiempo especificado por su `CacheDuration` propiedad. ObjectDataSource también tiene un [ `SqlCacheDependency` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que agrega una o varias dependencias de caché SQL a los datos almacenados en caché con el patrón:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Donde *databaseName* es el nombre de la base de datos como se especifica en el `name` atributo de la `<add>` elemento `Web.config`, y *tableName* es el nombre de la tabla de base de datos. Por ejemplo crear un origen ObjectDataSource que almacena en caché datos indefinidamente en función de una dependencia de caché SQL contra la s de Northwind `Products` de tabla, establezca la s ObjectDataSource `EnableCaching` propiedad `True` y su `SqlCacheDependency` propiedad NorthwindDB:Products.

> [!NOTE]
> Puede utilizar una dependencia de caché SQL *y* una expiración basado en tiempo estableciendo `EnableCaching` a `True`, `CacheDuration` para el intervalo de tiempo y `SqlCacheDependency` a los nombres de base de datos y tabla. ObjectDataSource expulsará sus datos cuando se alcanza la expiración del tiempo o cuando el sistema de sondeo detecta que la base de datos subyacente ha cambiado, lo que ocurra primero.


El control GridView en `SqlCacheDependencies.aspx` muestra los datos de dos tablas: `Products` y `Categories` (el producto s `CategoryName` campo se recupera a través de un `JOIN` en `Categories`). Por lo tanto, deseamos especificar dos dependencias de caché SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Configurar el origen ObjectDataSource para admitir el almacenamiento en caché con dependencias de caché de SQL en categorías y productos](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figura 9**: configurar el origen ObjectDataSource para compatibilidad con almacenamiento en caché utilizando SQL las dependencias de caché en `Products` y `Categories` ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image12.png))


Después de configurar el origen ObjectDataSource para admitir el almacenamiento en caché, volver a visitar la página a través de un explorador. Nuevamente, el evento de selección de texto que se desencadena debe aparecer en la primera visita de página, pero debería desaparecer cuando la paginación, ordenación o al hacer clic en los botones de edición o en Cancelar. Esto es porque después de cargan los datos en la caché de ObjectDataSource s, permanece allí hasta que el `Products` o `Categories` se modifican las tablas o los datos se actualizan a través del control GridView.

Después desencadena paginar a través de la cuadrícula y teniendo en cuenta la falta del evento cuando se selecciona texto, abra una nueva ventana del explorador y navegue hasta el tutorial de aspectos básicos de la edición, inserción y eliminación de la sección (`~/EditInsertDelete/Basics.aspx`). Actualizar el nombre o el precio de un producto. A continuación, de a la primera ventana de explorador, ver otra página de datos, ordenar la cuadrícula o haga clic en un botón de edición de fila s. Esta vez, el evento de selección desencadenado debe reaparecer, como la base de datos ha sido modificada (consulte la figura 10). Si no aparece el texto, espere unos instantes e inténtelo de nuevo. Recuerde que el servicio de sondeo está comprobando los cambios a la `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre cuando los datos subyacentes se actualizan y cuando se expulsan los datos en caché.


[![Modificación de la tabla Products extrae los datos del producto almacenada en caché](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figura 10**: modificación de la tabla Products extrae los datos del producto almacenado en caché ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Paso 6: Trabajar mediante programación con la`SqlCacheDependency`clase

El [almacenamiento en caché de datos en la arquitectura](caching-data-in-the-architecture-vb.md) tutorial examinamos las ventajas del uso de una capa de almacenamiento en caché independiente en la arquitectura en lugar de acoplando de manera precisa el almacenamiento en caché con ObjectDataSource. En este tutorial, creamos un `ProductsCL` clase demostrar trabajar mediante programación con la caché de datos. Para utilizar las dependencias de caché SQL en la capa de almacenamiento en caché, utilice el `SqlCacheDependency` clase.

Con el sistema de sondeo, una `SqlCacheDependency` objeto debe estar asociado con un par de base de datos y tabla determinado. El código siguiente, por ejemplo, se crea un `SqlCacheDependency` objeto basado en la base de datos Northwind s `Products` tabla:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Los dos parámetros de entrada al `SqlCacheDependency` constructor s son los nombres de base de datos y tabla, respectivamente. Al igual que con las operaciones de asignación ObjectDataSource `SqlCacheDependency` propiedad, el nombre de la base de datos utilizado es el mismo que el valor especificado en el `name` atributo de la `<add>` elemento `Web.config`. El nombre de tabla es el nombre real de la tabla de base de datos.

Para asociar un `SqlCacheDependency` con un elemento agregado a la caché de datos, utilice uno de los `Insert` sobrecargas del método que acepta una dependencia. El código siguiente agrega *valor* a la caché de datos de duración indefinida, pero lo asocia con un `SqlCacheDependency` en el `Products` tabla. En resumen, *valor* permanecerá en la memoria caché hasta que se expulse debido a restricciones de memoria o porque el sistema de sondeo ha detectado que la `Products` tabla ha cambiado desde que se almacenó en caché.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La capa de almacenamiento en caché de s `ProductsCL` clase actualmente almacena en caché los datos desde el `Products` tabla con una expiración basado en tiempo de 60 segundos. Permiten s actualizar esta clase para que utilice en su lugar las dependencias de caché SQL. El `ProductsCL` clase s `AddCacheItem` actualmente, método, que es responsable de agregar los datos a la memoria caché, contiene el código siguiente:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Actualizar este código para usar un `SqlCacheDependency` en lugar del objeto el `MasterCacheKeyArray` la dependencia de caché:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Para probar esta funcionalidad, agregue un control GridView a la página debajo existente `ProductsDeclarative` GridView. Establecer esta nueva s GridView `ID` a `ProductsProgrammatic` y, a través de su etiqueta inteligente, enlazarlo a un nuevo origen ObjectDataSource denominado `ProductsDataSourceProgrammatic`. Configurar el origen ObjectDataSource para usar el `ProductsCL` clase, estableciendo las listas desplegables en la instrucción SELECT y fichas de actualización `GetProducts` y `UpdateProduct`, respectivamente.


[![Configurar el origen ObjectDataSource para usar la clase ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figura 11**: configurar el origen ObjectDataSource que se usarán el `ProductsCL` clase ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image16.png))


[![Seleccione el método GetProducts en la lista de desplegable seleccione ficha s](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figura 12**: seleccione el `GetProducts` método desde la lista desplegable de seleccione ficha s ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image18.png))


[![Elija el método UpdateProduct por separado en la lista desplegable de actualización pestaña s](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figura 13**: elija el método UpdateProduct por separado en la lista desplegable de ficha actualización s ([haga clic aquí para ver imagen en tamaño completo](using-sql-cache-dependencies-vb/_static/image20.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Al igual que con la primera GridView agregado a esta página, quite todos los campos, pero `ProductName`, `CategoryName`, y `UnitPrice`y dar formato a estos campos como considere oportuno. En la etiqueta inteligente s GridView, active las casillas Enable Paging, Enable Sorting y Habilitar edición. Igual que con el `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio establecerá el `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` propiedad `original_{0}`. Para la característica de edición de GridView s para que funcione correctamente, establezca esta propiedad, realizar una copia en `{0}` (o quitar la asignación de propiedad de la sintaxis declarativa por completo).

Después de completar estas tareas, el marcado declarativo GridView y ObjectDataSource resultante debe ser similar al siguiente:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Para probar el código SQL dependencia de caché en la capa de almacenamiento en caché de establece un punto de interrupción en el `ProductCL` clase s `AddCacheItem` método y, a continuación, iniciar la depuración. Cuando visite `SqlCacheDependencies.aspx`, como los datos se solicita por primera vez y se coloca en la memoria caché, se alcanzará el punto de interrupción. A continuación, mover a otra página en el control GridView u ordenar una de las columnas. Esto hace que el control GridView a consultar sus datos, pero los datos deben encontrarse en la caché desde el `Products` no se modificó la tabla de base de datos. Si los datos varias veces no se encuentran en la memoria caché, asegúrese de que hay suficiente memoria disponible en el equipo y vuelva a intentarlo.

Después de la paginación a través de algunas páginas del control GridView, abra una segunda ventana del explorador y navegue hasta el tutorial de aspectos básicos de la edición, inserción y eliminación de la sección (`~/EditInsertDelete/Basics.aspx`). Actualizar un registro de la tabla Products y, a continuación, en la primera ventana de explorador, ver una nueva página o haga clic en uno de los encabezados de ordenación.

En este escenario verá dos cosas: ya sea el punto de interrupción se alcanzarán, que indica que los datos en caché se expulsan debido al cambio en la base de datos; o bien, el punto de interrupción no se tendrán en cuenta, lo que significa que `SqlCacheDependencies.aspx` ahora se muestran datos obsoletos. Si no se alcanza el punto de interrupción, es probable porque el servicio de sondeo no ha activado todavía desde que se ha cambiado los datos. Recuerde que el servicio de sondeo está comprobando los cambios a la `Products` tabla cada `pollTime` milisegundos, por lo que hay un retraso entre cuando los datos subyacentes se actualizan y cuando se expulsan los datos en caché.

> [!NOTE]
> Este retraso es más probable que aparezcan al editar uno de los productos a través del control GridView en `SqlCacheDependencies.aspx`. En el [almacenamiento en caché de datos en la arquitectura](caching-data-in-the-architecture-vb.md) tutorial se ha agregado el `MasterCacheKeyArray` dependencias para asegurarse de que los datos que se va a editar a través de la caché la `ProductsCL` clase s `UpdateProduct` se expulsó el método de la memoria caché. Sin embargo, esta dependencia de caché se sustituye al modificar el `AddCacheItem` método anteriormente en este paso y, por tanto, el `ProductsCL` clase seguirá mostrando los datos en caché hasta que el sistema de sondeo detecta el cambio a la `Products` tabla. Veremos cómo introducir el `MasterCacheKeyArray` la dependencia en el paso 7 de caché.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Paso 7: Asociar varias dependencias de un elemento almacenado en caché

Recuerde que el `MasterCacheKeyArray` dependencia de caché se utiliza para garantizar que *todas* datos relacionados con el producto se expulsan de la memoria caché cuando se actualiza cualquier elemento único asociado dentro de él. Por ejemplo, el `GetProductsByCategoryID(categoryID)` método cachés `ProductsDataTables` instancias para cada uno único *categoryID* valor. Si uno de estos objetos se expulsa, el `MasterCacheKeyArray` dependencia de caché garantiza que también se quitan los demás. Sin esta dependencia de caché cuando se modifican los datos almacenados en caché existe la posibilidad de que otros datos de productos almacenada en caché pueden no estar actualizados. Por lo tanto, lo importante que mantenemos la `MasterCacheKeyArray` dependencia de caché al usar dependencias de caché de SQL. Sin embargo, los datos de caché s `Insert` método solo se permite para un objeto de dependencia única.

Además, al trabajar con dependencias de caché de SQL, podemos necesitamos asociar varias tablas de base de datos como dependencias. Por ejemplo, el `ProductsDataTable` en caché el `ProductsCL` clase contiene los nombres de categoría y el proveedor para cada producto, pero la `AddCacheItem` método solo usa una dependencia en `Products`. En esta situación, si el usuario actualiza el nombre de una categoría o el proveedor, los datos del producto almacenada en caché se permanecen en la memoria caché y no estar actualizados. Por lo tanto, queremos hacer que los datos del producto almacenada en caché dependa no sólo el `Products` de tabla, pero en el `Categories` y `Suppliers` también las tablas.

El [ `AggregateCacheDependency` clase](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) proporciona un medio para asociar varias dependencias con un elemento de caché. Comience creando un `AggregateCacheDependency` instancia. A continuación, agregue el conjunto de dependencias mediante el `AggregateCacheDependency` s `Add` método. Al insertar el elemento en la caché de datos a partir de entonces, pase el `AggregateCacheDependency` instancia. Cuando *cualquier* de la `AggregateCacheDependency` cambian las dependencias de la instancia de s, el elemento en caché se expulsen.

La continuación muestra el código actualizado para el `ProductsCL` clase s `AddCacheItem` método. El método crea el `MasterCacheKeyArray` caché dependencia junto con `SqlCacheDependency` de objetos para el `Products`, `Categories`, y `Suppliers` tablas. Estas se combinan en una `AggregateCacheDependency` objeto denominado `aggregateDependencies`, que, a continuación, se pasa a la `Insert` método.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Pruebe este nuevo código de salida. Ahora cambia a la `Products`, `Categories`, o `Suppliers` los datos en caché se expulsen de hacer que las tablas. Además, el `ProductsCL` clase s `UpdateProduct` expulsa el método, que se llama cuando se edita un producto a través del control GridView, el `MasterCacheKeyArray` dependencia, lo que hace que el almacenado en caché de caché `ProductsDataTable` se expulsen y los datos que se van a volver a recuperar en la siguiente solicitud.

> [!NOTE]
> Dependencias de caché de SQL también pueden utilizarse con [la caché de resultados](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para ver una demostración de esta funcionalidad, consulte: [Using ASP.NET Output Caching with SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Resumen

Al almacenar en caché de la base de datos, los datos lo ideal es que permanecerán en la memoria caché hasta que se modifica en la base de datos. Con ASP.NET 2.0, se pueden crear y usar en escenarios declarativos y programáticos dependencias de caché de SQL. Uno de los desafíos con este enfoque está en la detección cuando se ha modificado los datos. Las versiones completas de Microsoft SQL Server 2005 proporcionan capacidades de notificación que se pueden alertar a una aplicación cuando ha cambiado el resultado de una consulta. Para las versiones anteriores de SQL Server y Express Edition de SQL Server 2005, un sistema de sondeo debe usarse en su lugar. Afortunadamente, la configuración de la infraestructura necesaria de sondeo es bastante sencilla.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Usar notificaciones de consulta en Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Creación de una notificación de consulta](https://msdn.microsoft.com/library/ms188669.aspx)
- [Almacenamiento en caché en ASP.NET con la `SqlCacheDependency` clase](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Herramienta de registro del servidor SQL de ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Información general de `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Marko Rangel, Teresa Murphy y Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-at-application-startup-vb.md)
