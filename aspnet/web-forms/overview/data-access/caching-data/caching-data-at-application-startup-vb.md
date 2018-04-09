---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Almacenamiento en caché de datos al iniciar la aplicación (VB) | Documentos de Microsoft
author: rick-anderson
description: En cualquier aplicación Web algunos datos se utilizará con frecuencia y algunos datos se utilizarán con poca frecuencia. Podemos mejorar el rendimiento de nuestro b de la aplicación de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: f8f322dae89480fc7ed5586d7f8eeb4c67d7839f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="caching-data-at-application-startup-vb"></a>Datos en caché al iniciar la aplicación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga de PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> En cualquier aplicación Web algunos datos se utilizará con frecuencia y algunos datos se utilizarán con poca frecuencia. Podemos mejorar el rendimiento de nuestra aplicación de ASP.NET mediante la carga de antemano los datos usados con frecuencia, una técnica conocida como. Este tutorial muestra un enfoque proactivo carga, que se usa para cargar datos en la memoria caché al iniciar la aplicación.


## <a name="introduction"></a>Introducción

Los dos tutoriales anteriores examinando el almacenamiento en caché datos de la capa de almacenamiento en caché y la presentación. En [almacenar datos en caché con el ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), analizamos usando la s ObjectDataSource características a los datos de caché en el nivel de presentación de caching. [Datos de la arquitectura del almacenamiento en caché](caching-data-in-the-architecture-vb.md) examina el almacenamiento en caché en una capa de almacenamiento en caché nueva e independiente. Ambos de estos tutoriales usan *carga reactiva* cuando se trabaja con la caché de datos. Al cargar reactiva, cada vez que se solicitan los datos, el sistema comprueba primero si lo s en la memoria caché. Si no es así, se toma los datos de origen, por ejemplo, la base de datos y, a continuación, se almacena en la memoria caché. La ventaja principal de carga reactiva es su facilidad de implementación. Uno de sus desventajas es su rendimiento desigual todas las solicitudes. Imagine una página que usa la capa de almacenamiento en caché del tutorial anterior para mostrar información del producto. Cuando esta página se visitado por primera vez o visitada por primera vez después de que los datos almacenados en caché se ha desalojado debido a restricciones de memoria o el vencimiento especificado tras haber sido informado, los datos se deben recuperar de la base de datos. Por lo tanto, estas solicitudes de los usuarios tardará más tiempo que las solicitudes de los usuarios que pueden proporcionarse en la memoria caché.

*Carga automático* proporciona una estrategia de administración de memoria caché alternativo que suaviza el rendimiento de todas las solicitudes mediante la carga de los datos almacenados en memoria caché antes de que s necesario. Normalmente, la carga automático utiliza algún proceso que periódicamente se comprueba o se notifica cuando se ha producido una actualización de los datos subyacentes. Este proceso, a continuación, actualiza la caché para mantener actualizado. Carga automático resulta especialmente útil si los datos subyacentes proceden de una conexión lenta de la base de datos, un servicio Web o algún otro origen de datos muy lenta. Pero este enfoque para cargar automático es más difícil de implementar, ya que requiere crear, administrar e implementar un proceso para comprobar los cambios y actualizar la memoria caché.

Otro tipo de carga automático y el tipo que se podrá explorar en este tutorial, está cargando datos en la memoria caché al iniciar la aplicación. Este enfoque es especialmente útil para almacenar en caché datos estáticos, como los registros en tablas de búsqueda de la base de datos.

> [!NOTE]
> Para obtener una visión más detallada de las diferencias entre cargar reactiva y proactiva, así como las listas de los profesionales de TI, inconvenientes y recomendaciones de implementación, consulte el [administrar el contenido de una memoria caché](https://msdn.microsoft.com/library/ms978503.aspx) sección de la [ Almacenamiento en caché de la Guía de arquitectura para aplicaciones de .NET Framework](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Paso 1: Determinar qué datos en memoria caché al iniciar la aplicación

Los ejemplos de almacenamiento en caché utilizando reactivos de carga se examina en el trabajo de dos tutoriales anteriores bien con datos que pueden cambiar de forma periódica y no toman exorbitantly mucho para generar. Pero si los datos almacenados en memoria caché nunca cambian, la expiración utilizada por carga reactiva es superflua. Del mismo modo, si los datos se almacenen en caché tardan un tiempo extremadamente largo para generar, se recuperan los usuarios cuyas solicitudes encontrar la Vaciar caché tendrán a tolerar una espera larga mientras que los datos subyacentes. Considere la posibilidad de almacenar en caché datos estáticos y los datos que se tarda demasiado tiempo a generar al iniciar la aplicación.

Aunque las bases de datos tienen muchos dinámicos, valores cambian con frecuencia, la mayoría tienen también una cantidad considerable de datos estáticos. Por ejemplo, prácticamente todos los modelos de datos tienen una o varias columnas que contienen un valor determinado de un conjunto fijo de opciones. A `Patients` tabla de base de datos podría tener un `PrimaryLanguage` columna, cuyo conjunto de valores podría estar inglés, español, francés, ruso, japonés y así sucesivamente. A menudo, estos tipos de columnas se implementan utilizando *tablas de búsqueda*. En lugar de almacenar la cadena en inglés o francés en el `Patients` tabla, una segunda tabla se crea que, normalmente, tiene dos columnas: un identificador único y una descripción de cadena - con un registro para cada valor posible. El `PrimaryLanguage` columna en el `Patients` tabla almacena el identificador único correspondiente en la tabla de búsqueda. En la figura 1, paciente idioma principal de Juan García s es inglés, mientras que s Ed Johnson es ruso.


![La tabla de idiomas es una tabla de búsqueda usa por la tabla de pacientes](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: el `Languages` tabla es una tabla de búsqueda utilizado por el `Patients` tabla


La interfaz de usuario para editar o crear un nuevo paciente incluye una lista desplegable de idiomas permitidos rellenado con los registros en la `Languages` tabla. Sin almacenamiento en caché, cada vez que esta interfaz visitó el sistema debe consultar el `Languages` tabla. Esto es innecesarios e innecesarios porque cambian los valores de tabla de búsqueda con muy poca frecuencia si alguna vez.

Se puede almacenar en caché el `Languages` datos mediante las mismas técnicas de carga reactiva examinadas en los tutoriales anteriores. Sin embargo, se carga reactiva, usa una fecha de expiración basada en el tiempo, que no es necesario para los datos de la tabla de búsqueda estática. Mientras que el almacenamiento en caché con carga reactiva es mejor que ningún almacenamiento en caché en absoluto, el mejor método sería proactivamente cargar los datos de tabla de búsqueda en la memoria caché al iniciar la aplicación.

En este tutorial, veremos cómo datos de la tabla de búsqueda de caché y otra información estática.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Paso 2: Examen de las distintas formas de almacenar datos en caché

Información puede utilizar caché mediante programación en una aplicación de ASP.NET mediante una variedad de métodos. Se ha visto cómo utilizar la caché de datos en los tutoriales anteriores. Como alternativa, los objetos pueden almacenarse mediante programación en caché mediante *miembros estáticos* o *estado de la aplicación*.

Cuando se trabaja con una clase, normalmente la clase debe en primer lugar se crea una instancia para poder acceder a sus miembros. Por ejemplo, para invocar un método de una de las clases de la capa de lógica empresarial, debemos primero creamos una instancia de la clase:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Antes de que podemos llamar *SomeMethod* o trabajar con *SomeProperty*, primero que debemos crear una instancia de la clase utilizando el `New` palabra clave. *SomeMethod* y *SomeProperty* están asociados a una instancia determinada. La duración de estos miembros se vincula a la duración de su objeto asociado. *Los miembros estáticos*, por otro lado, son las variables, propiedades y métodos que se comparten entre *todos los* instancias de la clase y, por lo tanto, tienen una duración siempre que la clase. Los miembros estáticos se indican mediante la palabra clave `Shared`.

Además de los miembros estáticos, datos pueden almacenarse en caché mediante el estado de la aplicación. Cada aplicación de ASP.NET mantiene una colección de nombre/valor s compartida todos los usuarios y las páginas de la aplicación. Puede tener acceso a esta colección mediante el [ `HttpContext` clase](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` propiedad](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)y usar desde una clase de código subyacente ASP.NET page s así:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

La caché de datos proporciona una cantidad API más enriquecida para almacenamiento en caché de datos, lo que ofrece mecanismos para basada en tiempo y dependencia caducados, las prioridades del elemento de caché y así sucesivamente. Con los miembros estáticos y estado de la aplicación, se deben agregar manualmente estas características por el desarrollador de la página. Al almacenar en caché datos al iniciar la aplicación para la duración de la aplicación, sin embargo, las ventajas de la memoria caché s de datos son discutible. En este tutorial se examinará código que usa las tres técnicas para almacenar en caché datos estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Paso 3: El almacenamiento en caché el`Suppliers`datos de tabla

El tablas de base de datos se ha implementado hasta la fecha de Northwind no incluyen ninguna tabla de búsqueda tradicional. Las cuatro tablas de datos implementan en nuestro DAL todas las tablas del modelo cuyos valores son no estáticos. En lugar de dedicar el tiempo para agregar un nuevo objeto DataTable a la capa DAL y, a continuación, una nueva clase y los métodos a la capa BLL, para este tutorial permite s simplemente supongamos que la `Suppliers` datos de la tabla s están estáticos. Por lo tanto, se pueden almacenar en caché estos datos al iniciarse la aplicación.

Para empezar, cree una nueva clase denominada `StaticCache.cs` en el `CL` carpeta.


![Crear la clase StaticCache.vb en la carpeta de CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: crear el `StaticCache.vb` clase en el `CL` carpeta


Tenemos que agregar un método que carga los datos en el inicio en el almacén de caché adecuada, así como métodos que devuelven datos de esta memoria caché.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

El código anterior utiliza una variable de miembro estático, `suppliers`, para almacenar los resultados de la `SuppliersBLL` clase s `GetSuppliers()` método, que se llama desde el `LoadStaticCache()` método. El `LoadStaticCache()` método está pensado para llamarse durante el inicio de s de la aplicación. Una vez que estos datos se han cargado al iniciar la aplicación, puede llamar cualquier página que necesita para trabajar con datos de proveedor la `StaticCache` clase s. `GetSuppliers()` método. Por lo tanto, la llamada a la base de datos para obtener los proveedores solo se produce una vez, al iniciarse la aplicación.

En lugar de usar una variable miembro estática como el almacén de caché, podríamos haber utilizado también el estado de aplicación o la caché de datos. El código siguiente muestra la clase reformó para usar el estado de la aplicación:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

En `LoadStaticCache()`, la información de proveedor se almacena en la variable de aplicación *clave*. Lo s se devuelve como el tipo adecuado (`Northwind.SuppliersDataTable`) desde `GetSuppliers()`. Mientras que el estado de aplicación son accesibles en las clases de código subyacente de páginas ASP.NET mediante `Application("key")`, en la arquitectura que debemos usar `HttpContext.Current.Application("key")` con el fin de obtener actual `HttpContext`.

Del mismo modo, la caché de datos puede utilizarse como un almacén de caché, como se muestra en el código siguiente:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Para agregar un elemento a la caché de datos y sin ninguna caducidad basadas en tiempo, use la `System.Web.Caching.Cache.NoAbsoluteExpiration` y `System.Web.Caching.Cache.NoSlidingExpiration` valores como parámetros de entrada. Esta sobrecarga en particular de la caché de datos s `Insert` seleccionó el método para que se puede especificar el *prioridad* de elemento de la caché. La prioridad se utiliza para determinar qué elementos se deben borrar de la memoria caché cuando hay poca memoria disponible. Aquí usamos la prioridad `NotRemovable`, lo que garantiza que este elemento en caché won t se eliminen.

> [!NOTE]
> Esta descarga tutorial s implementa la `StaticCache` clase utilizando el enfoque de variable de miembro estático. El código de las técnicas de caché de estado y los datos de aplicación está disponible en los comentarios en el archivo de clase.


## <a name="step-4-executing-code-at-application-startup"></a>Paso 4: Ejecutar código en el inicio de la aplicación

Para ejecutar código cuando se inicia por primera vez una aplicación web, es necesario crear un archivo especial denominado `Global.asax`. Este archivo puede contener controladores de eventos de aplicación-, sesión:, y eventos de nivel de solicitud y aquí es donde podemos agregar código que se ejecutará cada vez que inicia la aplicación.

Agregar el `Global.asax` archivo para el directorio raíz de s de aplicación web, haga doble clic en el nombre del proyecto de sitio Web en Visual Studio s el Explorador de soluciones y elija Agregar nuevo elemento. En el cuadro de diálogo Agregar nuevo elemento, seleccione el tipo de elemento de la clase de aplicación Global y, a continuación, haga clic en el botón Agregar.

> [!NOTE]
> Si ya tiene un `Global.asax` archivo en el proyecto, el tipo de elemento no se mostrarán en el cuadro de diálogo Agregar nuevo elemento de clase de aplicación Global.


[![Agregue el archivo Global.asax a su directorio de raíz de s de la aplicación Web](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: agregar la `Global.asax` archivo a una aplicación Web s directorio raíz ([haga clic aquí para ver la imagen a tamaño completo](caching-data-at-application-startup-vb/_static/image5.png))


El valor predeterminado `Global.asax` plantilla de archivo incluye cinco métodos dentro de un servidor `<script>` etiqueta:

- **`Application_Start`** se ejecuta cuando se inicia por primera vez la aplicación web
- **`Application_End`** se ejecuta cuando se está cerrando la aplicación
- **`Application_Error`** se ejecuta cada vez que una excepción no controlada llega a la aplicación
- **`Session_Start`** se ejecuta cuando se crea una nueva sesión
- **`Session_End`** se ejecuta cuando una sesión está expirada o se abandonó el proceso

El `Application_Start` controlador de eventos se llama solo una vez durante un ciclo de vida de aplicación s. La aplicación inicia la primera vez un recurso ASP.NET se solicita la aplicación y continúa ejecutándose hasta que se reinicie la aplicación, lo cual puede suceder si modifica el contenido de la `/Bin` carpeta, modificar `Global.asax`, modificar el contenido en el `App_Code` carpeta, o modificar el `Web.config` archivo, entre otras causas. Hacer referencia a [información general sobre el ciclo de vida de aplicaciones ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) para obtener una explicación más detallada en el ciclo de vida de la aplicación.

Para estos tutoriales únicamente se necesita agregar código a la  `Application_Start` /método siguiente, por lo que no dude en quitar las demás. En `Application_Start`, basta con llamar a la `StaticCache` clase s. `LoadStaticCache()` método, que se cargarán y almacenar en caché la información del proveedor:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

S todo es a él. Al iniciar la aplicación, el `LoadStaticCache()` método agarre la información del proveedor de la capa BLL y almacenarlo en una variable miembro estática (o almacenar la memoria caché se ha terminado de selección en la `StaticCache` clase). Para comprobar este comportamiento, establezca un punto de interrupción la `Application_Start` (método) y ejecute la aplicación. Tenga en cuenta que el punto de interrupción en el inicio de la aplicación. Las solicitudes posteriores, sin embargo, no se realiza el `Application_Start` método se debe ejecutar.


[![Usar un punto de interrupción para comprobar que el controlador de eventos Application_Start es que se está ejecutando](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: usar un punto de interrupción para comprobar que la `Application_Start` controlador de eventos es que se está ejecutando ([haga clic aquí para ver la imagen a tamaño completo](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Si no se produce la `Application_Start` punto de interrupción al primero iniciar la depuración, es que ya ha iniciado la aplicación. Forzar la aplicación se reinicie modificando su `Global.asax` o `Web.config` archivos y, a continuación, vuelva a intentarlo. Se puede simplemente agregar (o quitar) una línea en blanco al final de uno de estos archivos rápidamente reiniciar la aplicación.


## <a name="step-5-displaying-the-cached-data"></a>Paso 5: Mostrar los datos en caché

En este momento la `StaticCache` clase tiene una versión de los datos de proveedor almacenado en caché en el inicio de la aplicación que se puede tener acceso a través de su `GetSuppliers()` método. Para trabajar con estos datos de la capa de presentación, podemos usar un ObjectDataSource o invocar mediante programación el `StaticCache` clase s. `GetSuppliers()` método de una clase de código subyacente de la página s ASP.NET. Permiten s mirar mediante los controles ObjectDataSource y GridView para mostrar la información de proveedor almacenados en caché.

Comience abriendo la `AtApplicationStartup.aspx` página en el `Caching` carpeta. Arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Suppliers`. A continuación, desde GridView etiqueta inteligente s optar por crear un nuevo origen ObjectDataSource denominado `SuppliersCachedDataSource`. Configurar el ObjectDataSource para usar el `StaticCache` clase s. `GetSuppliers()` método.


[![Configurar el ObjectDataSource para utilizar la clase StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configurar el ObjectDataSource para usar el `StaticCache` clase ([haga clic aquí para ver la imagen a tamaño completo](caching-data-at-application-startup-vb/_static/image11.png))


[![Utilice el método GetSuppliers() para recuperar los datos de proveedor almacenados en caché](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: Use la `GetSuppliers()` método para recuperar los datos de proveedor almacenado en memoria caché ([haga clic aquí para ver la imagen a tamaño completo](caching-data-at-application-startup-vb/_static/image14.png))


Después de completar el asistente, Visual Studio agregará automáticamente BoundFields para cada uno de los campos de datos en `SuppliersDataTable`. El marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figura 7 muestra la página cuando se ve mediante un explorador. El resultado es el mismo hubieran se extraen los datos de las operaciones de asignación BLL `SuppliersBLL` clase, pero utilizando la `StaticCache` clase devuelve los datos de proveedor como almacenado en caché al iniciar la aplicación. Puede establecer puntos de interrupción la `StaticCache` clase s. `GetSuppliers()` método para comprobar este comportamiento.


[![Los datos de proveedor almacenado en memoria caché se muestra en un control GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: el almacenado en memoria caché de datos de proveedor se muestra en un control GridView ([haga clic aquí para ver la imagen a tamaño completo](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Resumen

La mayoría de cada modelo de datos contiene una cantidad considerable de datos estáticos, que normalmente se implementa en forma de tabla de búsqueda. Dado que esta información es estática, allí s ningún motivo para tener acceso a la base de datos continuamente cada vez que esta información se debe mostrar. Además, debido a su naturaleza estática, al almacenar en caché los datos allí s sin necesidad de una fecha de expiración. En este tutorial, hemos visto cómo utilizar estos datos y almacenarla en la caché en la caché de datos, el estado de la aplicación y a través de una variable de miembro estático. Esta información se almacena en caché al iniciar la aplicación y permanece en la caché durante el ciclo de vida de aplicación s.

En este tutorial y los dos últimos, se ha buscado en almacenamiento en caché datos para la duración de la duración de s de la aplicación, así como usar caducados basado en tiempo. Al almacenar en caché datos de la base de datos, sin embargo, una expiración basada en el tiempo puede ser ni mucho menos ideal. En lugar de forma periódica el vaciado de la memoria caché, sería óptimo para expulsar solo el elemento en caché cuando se modifican los datos de la base de datos subyacente. Este ideal es posible mediante el uso de dependencias de caché SQL, que examinaremos en nuestro tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Teresa Murphy y Zack Jones. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-vb.md)
> [Siguiente](using-sql-cache-dependencies-vb.md)
