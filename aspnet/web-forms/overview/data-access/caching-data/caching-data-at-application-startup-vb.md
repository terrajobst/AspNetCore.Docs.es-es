---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Almacenamiento en caché de datos al iniciarse la aplicación (VB) | Microsoft Docs
author: rick-anderson
description: En cualquier aplicación Web algunos datos se usará con frecuencia y algunos datos se utilizarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación de ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: 9d997cb0af8487b2a9d849f24f3bff8df8611f6d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829456"
---
<a name="caching-data-at-application-startup-vb"></a>Datos en caché al iniciarse la aplicación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> En cualquier aplicación Web algunos datos se usará con frecuencia y algunos datos se utilizarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación de ASP.NET mediante la carga de antemano los datos usados con frecuencia, una técnica conocida como. Este tutorial muestra un enfoque de carga automático, que se usa para cargar datos en la memoria caché al iniciarse la aplicación.


## <a name="introduction"></a>Introducción

Los dos tutoriales anteriores examinado en caché los datos en la presentación y capas de almacenamiento en caché. En [datos de almacenamiento en caché con ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), contemplamos mediante las características en caché los datos en la capa de presentación de caching la s de ObjectDataSource. [Almacenamiento en caché de datos en la arquitectura](caching-data-in-the-architecture-vb.md) examinar el almacenamiento en caché en una capa nueva e independiente del almacenamiento en caché. Ambos de estos tutoriales usan *carga reactiva* para trabajar con la caché de datos. Con la carga reactiva, cada vez que se solicitan los datos, el sistema comprueba primero si lo s en la memoria caché. Si no, toma los datos desde el origen original, como la base de datos y, a continuación, almacena en la memoria caché. La ventaja principal de la carga reactiva es su facilidad de implementación. Uno de sus desventajas es su rendimiento desigual entre solicitudes. Imagine una página que usa la capa de almacenamiento en caché del tutorial anterior para mostrar información de producto. Cuando esta página se visitó por primera vez o visitada por primera vez después de los datos en caché se ha desalojado debido a restricciones de memoria o la expiración especificada que se ha alcanzado, se deben recuperar los datos de la base de datos. Por lo tanto, estas solicitudes de usuarios tardará más que las solicitudes de los usuarios que pueden proporcionarse por la memoria caché.

*Cargando proactiva* proporciona una estrategia de administración de memoria caché alternativo que suaviza el rendimiento en las solicitudes al cargar los datos en caché antes de que s necesarios. Normalmente, la carga proactiva usa algún proceso que periódicamente se comprueba o se notifica cuando se ha producido una actualización de los datos subyacentes. Este proceso, a continuación, actualiza la caché para mantener actualizados. La carga proactiva es especialmente útil si los datos subyacentes provienen de una conexión lenta de la base de datos, un servicio Web o algún otro origen de datos especialmente lenta. Pero este enfoque de carga automática es más difícil de implementar, ya que requiere crear, administrar e implementar un proceso para comprobar los cambios y actualizar la memoria caché.

Otro tipo de carga automática y el tipo que vamos a explorar en este tutorial, está cargando datos en la memoria caché al iniciarse la aplicación. Este enfoque es especialmente útil para almacenar en caché datos estáticos, como los registros de base de datos en tablas de búsqueda.

> [!NOTE]
> Para una información más detallada sobre las diferencias entre la carga proactiva y reactiva, así como listas de ventajas, desventajas y recomendaciones para la implementación, consulte el [administrar el contenido de una memoria caché](https://msdn.microsoft.com/library/ms978503.aspx) sección de la [ Almacenamiento en caché de la Guía de arquitectura para aplicaciones de .NET Framework](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Paso 1: Determinar qué datos en memoria caché al iniciarse la aplicación

Los ejemplos de almacenamiento en caché mediante la carga reactivo hemos visto en el trabajo de dos tutoriales anteriores bien con datos que se pueden cambiar periódicamente y que no tardará mucho exorbitantly para generar. Pero si los datos en caché nunca cambian, la expiración utilizada por la carga reactiva es superflua. Del mismo modo, si los datos se almacenen en caché tardan un tiempo extremadamente largo para generar, se recupera los usuarios cuyas solicitudes se encuentra vacía de la memoria caché tendrán a tolerar una espera prolongada mientras los datos subyacentes. Considere la posibilidad de almacenar en caché datos estáticos y datos que se tarda demasiado tiempo a generar al iniciarse la aplicación.

Mientras que las bases de datos tienen muchas dinámicos, valores cambian con frecuencia, la mayoría también tienen una gran cantidad de datos estáticos. Por ejemplo, prácticamente todos los modelos de datos tienen una o varias columnas que contienen un valor determinado de un conjunto fijo de opciones. Un `Patients` tabla de base de datos podría tener un `PrimaryLanguage` columna, cuyo conjunto de valores podría ser, por ejemplo, inglés, español, francés, ruso, japonés y así sucesivamente. A menudo, estos tipos de columnas se implementan mediante *tablas de búsqueda*. En lugar de almacenar la cadena de inglés o francés en el `Patients` tabla, una segunda tabla se crea que, normalmente, tiene dos columnas: un identificador único y una descripción de cadena - con un registro para cada valor posible. El `PrimaryLanguage` columna en el `Patients` tabla almacena el identificador único correspondiente en la tabla de búsqueda. En la figura 1, paciente idioma principal de John Doe s es el inglés, aunque Ed Johnson s es el ruso.


![La tabla de lenguajes es una tabla de búsqueda usa por la tabla Patients](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: el `Languages` tabla es una tabla de búsqueda utilizado por el `Patients` tabla


La interfaz de usuario para editar o crear un nuevo paciente incluye una lista desplegable de idiomas permitidos rellenada con los registros en el `Languages` tabla. Sin almacenamiento en caché, cada vez que esta interfaz visitó el sistema debe consultar la `Languages` tabla. Esto es derrochador e innecesario, ya que los valores de tabla de búsqueda con muy poca frecuencia, cambian si alguna vez.

Nos podríamos almacenar en caché el `Languages` datos mediante las mismas técnicas de carga reactivo examinadas en los tutoriales anteriores. Cargando reactiva, sin embargo, utiliza una expiración basada en el tiempo, que no es necesario para los datos de la tabla de búsqueda estática. Aunque el almacenamiento en caché mediante la carga reactiva sería mejor que ningún almacenamiento en caché en absoluto, el mejor enfoque sería proactivamente cargar los datos de tabla de búsqueda en la memoria caché al iniciarse la aplicación.

En este tutorial veremos cómo los datos de la tabla de búsqueda de caché y otra información estática.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Paso 2: Examen de las distintas formas de almacenar datos en caché

Puede almacenar mediante programación información en una aplicación ASP.NET con una variedad de enfoques. Se ve ya se ha visto cómo usar la caché de datos en los tutoriales anteriores. Como alternativa, los objetos pueden almacenarse mediante programación en caché mediante *miembros estáticos* o *estado de la aplicación*.

Cuando se trabaja con una clase, normalmente la clase primero se debe crear instancias poder acceder a sus miembros. Por ejemplo, para invocar un método de una de las clases de la capa de lógica empresarial, debemos primero creamos una instancia de la clase:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Antes de que podemos llamar *SomeMethod* o trabajar con *SomeProperty*, primero debemos crear una instancia de la clase utilizando el `New` palabra clave. *SomeMethod* y *SomeProperty* están asociados con una instancia determinada. La duración de estos miembros se vincula a la duración de su objeto asociado. *Los miembros estáticos*, por otro lado, están las variables, propiedades y métodos que se comparten entre *todas* instancias de la clase y, por lo tanto, tienen una duración siempre y cuando la clase. Los miembros estáticos se indican mediante la palabra clave `Shared`.

Además de los miembros estáticos, datos pueden almacenarse en caché con el estado de la aplicación. Cada aplicación de ASP.NET mantiene una colección de nombre/valor que s que se comparte entre todos los usuarios y las páginas de la aplicación. Esta colección se puede acceder mediante el [ `HttpContext` clase](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` propiedad](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)y usar de una clase de código subyacente ASP.NET página s así:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La caché de datos proporciona una mucho más rica API para almacenar en caché los datos y ofrecen mecanismos para basados en tiempo y dependencia caducados, las prioridades del elemento de caché y así sucesivamente. Con los miembros estáticos y estado de la aplicación, estas características deben agregarse manualmente por el desarrollador de páginas. Al almacenar en caché datos al iniciarse la aplicación durante la vigencia de la aplicación, sin embargo, las ventajas de s de caché de datos son discutible. En este tutorial, echaremos un vistazo al código que usa las tres técnicas para almacenar en caché datos estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Paso 3: Almacenamiento en caché el`Suppliers`datos de tabla

La base de datos de tablas se ve implementado hasta la fecha de Northwind no incluyen ninguna tabla de búsqueda tradicional. Las cuatro tablas de datos implementan en nuestro DAL todas las tablas del modelo cuyos valores son no estáticos. En lugar de dedicar el tiempo para agregar una nueva DataTable a la capa DAL y, a continuación, una nueva clase y métodos a la capa BLL, para este tutorial permite s simplemente fingir que el `Suppliers` datos de la tabla s están estáticos. Por lo tanto, podríamos almacenaremos en caché estos datos al iniciarse la aplicación.

Para empezar, cree una nueva clase denominada `StaticCache.cs` en el `CL` carpeta.


![Crear la clase StaticCache.vb en la carpeta de CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: crear el `StaticCache.vb` clase en el `CL` carpeta


Necesitamos agregar un método que carga los datos en el inicio en el almacén de caché adecuada, así como los métodos que devuelven datos de esta caché.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

El código anterior usa una variable de miembro estático, `suppliers`, para almacenar los resultados de la `SuppliersBLL` clase s `GetSuppliers()` método, que se llama desde el `LoadStaticCache()` método. El `LoadStaticCache()` método está pensado para llamarse durante el inicio de s de la aplicación. Una vez que estos datos se han cargado al iniciarse la aplicación, puede llamar cualquier página que necesita para trabajar con datos de proveedor la `StaticCache` clase s `GetSuppliers()` método. Por lo tanto, la llamada a la base de datos para obtener los proveedores solo se produce una vez, al iniciar la aplicación.

En lugar de usar una variable miembro estática como el almacén de caché, podríamos haber usado también el estado de aplicación o la caché de datos. El código siguiente muestra la clase reformó para usar el estado de la aplicación:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

En `LoadStaticCache()`, se almacena la información del proveedor a la variable de aplicación *clave*. Se devolverá el tipo apropiado (`Northwind.SuppliersDataTable`) desde `GetSuppliers()`. Mientras el estado de la aplicación puede tener acceso en las clases de código subyacente de las páginas ASP.NET mediante `Application("key")`, en la arquitectura debemos usar `HttpContext.Current.Application("key")` con el fin de obtener actual `HttpContext`.

Del mismo modo, la caché de datos puede usarse como un almacén de caché, como se muestra en el código siguiente:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Para agregar un elemento a la caché de datos con sin expiración basado en tiempo, use el `System.Web.Caching.Cache.NoAbsoluteExpiration` y `System.Web.Caching.Cache.NoSlidingExpiration` valores como parámetros de entrada. Esta sobrecarga en particular de la caché de datos s `Insert` método se ha seleccionado para que pudiéramos especificar la *prioridad* de elemento de la caché. La prioridad se utiliza para determinar qué elementos de borrado de la memoria caché cuando hay poca memoria disponible. Aquí usamos la prioridad `NotRemovable`, lo que garantiza que este elemento en caché ganó t se eliminen sus.

> [!NOTE]
> Esta descarga tutorial s implementa la `StaticCache` clase mediante el enfoque de variable de miembro estático. El código de las técnicas de caché de estado y los datos de aplicación está disponible en los comentarios en el archivo de clase.


## <a name="step-4-executing-code-at-application-startup"></a>Paso 4: Ejecutar código en el inicio de la aplicación

Para ejecutar código cuando una aplicación web se inicia por primera vez, es necesario crear un archivo especial denominado `Global.asax`. Este archivo puede contener controladores de eventos de aplicación-, sesión-, y los eventos de nivel de solicitud y aquí es donde podemos agregar código que se ejecutará cada vez que se inicia la aplicación.

Agregar el `Global.asax` archivo al directorio raíz web application s, con el botón secundario en el nombre del proyecto de sitio Web en Visual Studio s el Explorador de soluciones y elija Agregar nuevo elemento. En el cuadro de diálogo Agregar nuevo elemento, seleccione el tipo de elemento de la clase de aplicación Global y, a continuación, haga clic en el botón Agregar.

> [!NOTE]
> Si ya tiene un `Global.asax` archivo del proyecto, el tipo de elemento no aparecerá en el cuadro de diálogo Agregar nuevo elemento de clase de aplicación Global.


[![Agregar el archivo Global.asax al directorio raíz Web Application s](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: agregar el `Global.asax` archivo s directorio raíz de la aplicación Web ([haga clic aquí para ver imagen en tamaño completo](caching-data-at-application-startup-vb/_static/image5.png))


El valor predeterminado `Global.asax` plantilla de archivo incluye cinco métodos dentro de un servidor `<script>` etiqueta:

- **`Application_Start`** se ejecuta cuando se inicia por primera vez la aplicación web
- **`Application_End`** se ejecuta cuando se está cerrando la aplicación
- **`Application_Error`** ejecuta cada vez que una excepción no controlada llega a la aplicación
- **`Session_Start`** se ejecuta cuando se crea una nueva sesión
- **`Session_End`** se ejecuta cuando una sesión ha caducado o se ha abandonado

El `Application_Start` se llama al controlador de eventos sólo una vez durante un ciclo de vida de aplicación s. La aplicación inicia la primera vez un recurso de ASP.NET se solicita desde la aplicación y continúa ejecutándose hasta que se reinicie la aplicación, lo que puede suceder si modifica el contenido de la `/Bin` carpeta, modificar `Global.asax`, modificar el contenido en el `App_Code` carpeta, o modificar el `Web.config` archivo, entre otras causas. Consulte [información general sobre el ciclo de vida de aplicaciones ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) para una explicación más detallada sobre el ciclo de vida de la aplicación.

Estos tutoriales sólo tenemos que agregar código a la `Application_Start` método, por lo que si quiere, puede quitar las demás. En `Application_Start`, basta con llamar a la `StaticCache` clase s `LoadStaticCache()` método, que se cargará y almacenar en caché la información del proveedor:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

¡Y ya está todo s! Al iniciarse la aplicación, el `LoadStaticCache()` método tomará la información del proveedor de BLL y almacenarlo en una variable de miembro estático (o cualquier caché almacenar acabé por uso en el `StaticCache` clase). Para comprobar este comportamiento, establezca un punto de interrupción el `Application_Start` método y ejecutar la aplicación. Tenga en cuenta que el punto de interrupción tras el inicio de la aplicación. Hacen que las solicitudes posteriores, sin embargo, el `Application_Start` ejecución del método.


[![Usar un punto de interrupción para verificar que el controlador de eventos Application_Start es que se está ejecutando](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: usar un punto de interrupción para comprobar que la `Application_Start` controlador de eventos es que se está ejecutando ([haga clic aquí para ver imagen en tamaño completo](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Si no se produce la `Application_Start` punto de interrupción al primero iniciar la depuración, es que ya ha iniciado la aplicación. Forzar la aplicación se reinicie modificando su `Global.asax` o `Web.config` archivos y, a continuación, vuelva a intentarlo. Puede simplemente agregar (o quitar) una línea en blanco al final de uno de estos archivos rápidamente reiniciar la aplicación.


## <a name="step-5-displaying-the-cached-data"></a>Paso 5: Mostrar los datos en caché

En este momento la `StaticCache` clase tiene una versión de los datos de proveedor almacenado en caché al iniciarse la aplicación que se puede acceder a través de su `GetSuppliers()` método. Para trabajar con estos datos desde la capa de presentación, podemos usar un origen ObjectDataSource o invocar mediante programación el `StaticCache` clase s `GetSuppliers()` método de una clase de código subyacente de s de la página ASP.NET. Permiten s una mirada al uso de los controles de ObjectDataSource y GridView para mostrar la información almacenada en caché del proveedor.

Comience abriendo la `AtApplicationStartup.aspx` página en el `Caching` carpeta. Arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Suppliers`. A continuación, desde el control GridView etiqueta inteligente de s optar por crear un nuevo origen ObjectDataSource denominado `SuppliersCachedDataSource`. Configurar el origen ObjectDataSource para usar el `StaticCache` clase s `GetSuppliers()` método.


[![Configurar el origen ObjectDataSource para usar la clase StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configurar el origen ObjectDataSource para usar el `StaticCache` clase ([haga clic aquí para ver imagen en tamaño completo](caching-data-at-application-startup-vb/_static/image11.png))


[![Utilice el método GetSuppliers() para recuperar los datos de proveedor almacenado en caché](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: Use el `GetSuppliers()` método para recuperar los datos de proveedor almacenado en caché ([haga clic aquí para ver imagen en tamaño completo](caching-data-at-application-startup-vb/_static/image14.png))


Después de completar el asistente, Visual Studio agregará automáticamente BoundFields para cada uno de los campos de datos en `SuppliersDataTable`. El marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figura 7 muestra la página cuando se ve mediante un explorador. El resultado es el mismo si hubiéramos extraemos los datos del s BLL `SuppliersBLL` clase, pero utilizando el `StaticCache` clase devuelve los datos de proveedor como en caché al iniciarse la aplicación. Puede establecer puntos de interrupción el `StaticCache` clase s `GetSuppliers()` método para comprobar este comportamiento.


[![Los datos de proveedor almacenado en caché se muestra en un control GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: los datos de proveedor almacenado en caché se muestra en un control GridView ([haga clic aquí para ver imagen en tamaño completo](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Resumen

La mayoría de cada modelo de datos contiene una gran cantidad de datos estáticos, que normalmente se implementa en forma de tablas de búsqueda. Dado que esta información es estática, allí s ninguna razón para tener acceso a la base de datos continuamente cada vez que esta información debe mostrarse. Además, debido a su naturaleza estática, al almacenar en caché los datos allí s sin necesidad de una fecha de expiración. En este tutorial hemos visto cómo tomar estos datos y lo almacena en caché en la caché de datos, el estado de la aplicación y a través de una variable de miembro estático. Esta información se almacena en caché al iniciarse la aplicación y permanece en la caché durante la vigencia de s de la aplicación.

En este tutorial y los dos últimos, se ve examinado de almacenamiento en caché datos para la duración de la duración de s aplicación así como el uso expirados de duración definida. Sin embargo, al almacenar en caché de la base de datos, una basada en tiempo de expiración puede ser ideal. En lugar de forma periódica, vaciar la memoria caché, es óptimo para expulsar solo el elemento en caché cuando se modifica la base de datos subyacente. Este ideal es posible mediante el uso de dependencias de caché de SQL, que se examinarán en nuestro tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y Zack Jones. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-vb.md)
> [Siguiente](using-sql-cache-dependencies-vb.md)
