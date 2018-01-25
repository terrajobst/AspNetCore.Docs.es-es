---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: "Mostrar una página de Error personalizada (VB) | Documentos de Microsoft"
author: rick-anderson
description: "¿Lo que ve el usuario cuando se produce un error en tiempo de ejecución en una aplicación web ASP.NET? La respuesta depende del sitio Web &lt;customErrors&gt; configuración..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 28f4c95e1578c5c91cfa1a21af2b4720ba7b286c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="displaying-a-custom-error-page-vb"></a>Mostrar una página de Error personalizada (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> ¿Lo que ve el usuario cuando se produce un error en tiempo de ejecución en una aplicación web ASP.NET? La respuesta depende del sitio Web &lt;customErrors&gt; configuración. De forma predeterminada, los usuarios ven una pantalla amarilla las proclamando que se ha producido un error en tiempo de ejecución. Este tutorial muestra cómo personalizar esta configuración a la página de errores personalizados de mostrar un estéticamente-agradable que coincida con la apariencia de su sitio.


## <a name="introduction"></a>Introducción

En un mundo perfecto no habría ningún error de tiempo de ejecución. Los programadores que habría que escribir código demasiado un error y con la validación de entrada de usuario sólida y externos recursos como servidores de base de datos y los servidores de correo electrónico nunca pasó sin conexión. Por supuesto, en realidad los errores son inevitables. Las clases de .NET Framework indican un error iniciando una excepción. Por ejemplo, una llamada a un objeto SqlConnection método Open del objeto establece una conexión a la base de datos especificada por una cadena de conexión. Sin embargo, si la base de datos está inactivo o si las credenciales en la cadena de conexión no son válidas, a continuación, el método Open produce un `SqlException`. Las excepciones pueden controlarse mediante el uso de `Try/Catch/Finally` bloques. Si el código dentro de un `Try` bloque produce una excepción, el control se transfiere al bloque catch adecuado donde el desarrollador puede intentar recuperarse del error. Si no hay ningún bloque catch coincidente, o si el código que produjo la excepción no está en un bloque try, la excepción percolates la pila de llamadas en search de `Try/Catch/Finally` bloques.

Si la excepción se propaga hasta el tiempo de ejecución ASP.NET sin que se controla, el [ `HttpApplication` clase](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)del [ `Error` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) se genera y se ignora el número *página de error*  se muestra. De forma predeterminada, ASP.NET muestra una página de error que lo denomina afectuosamente se conoce como el [amarillo pantalla de muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Hay dos versiones de la YSOD: muestra los detalles de la excepción, un seguimiento de pila y otra información útil a los desarrolladores depurar la aplicación (consulte **figura 1**); la otra indica simplemente que se produjo un error de tiempo de ejecución (vea  **Figura 2**).

Los detalles de excepción YSOD es muy útil para los desarrolladores depurar la aplicación, pero que muestra un YSOD a los usuarios finales es tacky y profesional. En su lugar, los usuarios finales deben tener cuidados para una página de error que mantiene la apariencia del sitio con texto más fácil de usar que describe la situación. Lo bueno es que es bastante fácil crear dicha página de error personalizado. Este tutorial comienza con un vistazo a ASP. Páginas de error distinto de NET. A continuación, se muestra cómo configurar la aplicación web para mostrar a los usuarios una página de error personalizado en el caso de un error.

### <a name="examining-the-three-types-of-error-pages"></a>Examen de los tres tipos de páginas de errores

Se muestra cuando se produce una excepción no controlada en una aplicación de ASP.NET uno de tres tipos de páginas de error:

- La página de error de excepción detalles pantalla amarillo de muerte,
- La página de error en tiempo de ejecución Error pantalla amarillo de muerte, o
- Una página de error personalizada

Los programadores de la página de error son más familiarizado con son el YSOD de detalles de excepción. De forma predeterminada, esta página se muestra a los usuarios que están visitando localmente y, por tanto, es la página que se ve cuando se produce un error al probar el sitio en el entorno de desarrollo. Como su nombre implica, el YSOD de detalles de excepción proporciona detalles sobre la excepción: el tipo, el mensaje y el seguimiento de pila. Es más, si se produjo la excepción mediante código de clase de código subyacente de la página ASP.NET y si la aplicación está configurada para la depuración del YSOD de detalles de excepción también mostrará esta línea de código (y unas pocas líneas de código encima y debajo de ella).

**Figura 1** muestra la página YSOD de detalles de excepción. Tenga en cuenta la dirección URL en la ventana del explorador dirección: `http://localhost:62275/Genre.aspx?ID=foo`. Recuerde que la `Genre.aspx` página enumera las reseñas de libros de un género determinado. Tiene los siguientes requisitos `GenreId` valor (un `uniqueidentifier`) debe pasar la cadena de consulta; por ejemplo, es la dirección URL apropiada para ver las revisiones de ficción `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Si no`uniqueidentifier` se pasa el valor de a través de la cadena de consulta (por ejemplo, "foo") se produce una excepción.

> [!NOTE]
> Para reproducir este error en la aplicación de demostración web disponible para su descarga puede cualquier visita `Genre.aspx?ID=foo` directamente o haga clic en el vínculo "Generar un Error en tiempo de ejecución" en `Default.aspx`.


Tenga en cuenta la información de excepción que se presentan en **figura 1**. El mensaje de excepción, "Error de conversión al convertir de una cadena de caracteres a uniqueidentifier" está presente en la parte superior de la página. El tipo de la excepción, `System.Data.SqlClient.SqlException`, se muestra, así. También es el seguimiento de pila.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Figura 1**: detalles de la excepción YSOD incluye información sobre la excepción  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image3.png))

El otro tipo de YSOD es el YSOD de Error en tiempo de ejecución y se muestra en **figura 2**. El YSOD de Error en tiempo de ejecución informa al visitante que se ha producido un error en tiempo de ejecución, pero no incluir toda la información sobre la excepción que se produjo. (Sin embargo,, proporcionan instrucciones sobre cómo hacer visibles los detalles del error modificando la `Web.config` archivo, que forma parte de lo que hace que estos un YSOD parezca poco profesional.)

De forma predeterminada, el YSOD de Error en tiempo de ejecución se muestra a los usuarios que visitan de forma remota (a través de http://www.yoursite.com), como pone de manifiesto por la dirección URL en la barra de direcciones del explorador de **figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Las dos pantallas YSOD diferentes existen porque los desarrolladores están interesados en conocer los detalles del error, pero dicha información no se debe mostrar en un sitio en vivo tal y como puede revelar vulnerabilidades de seguridad u otra información confidencial a cualquier persona que visita el sitio.

> [!NOTE]
> Si está siguiendo y está utilizando DiscountASP.NET como su host de web, puede observar que la YSOD de Error en tiempo de ejecución no se muestran al visitar el sitio activo. Esto es porque DiscountASP.NET tiene sus servidores configurados para mostrar el YSOD de detalles de excepción de forma predeterminada. Lo bueno es que puede invalidar este comportamiento predeterminado mediante la adición de un `<customErrors>` sección a su `Web.config` archivo. La sección "Configurar que Error es mostrar la página" examina la `<customErrors>` sección detalladamente.


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Figura 2**: el Error en tiempo de ejecución YSOD no incluye los detalles del Error  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image6.png))

El tercer tipo de página de error es la página de error personalizada, que es una página web que cree. La ventaja de una página de error personalizada es que tiene un control completo sobre la información que se muestra al usuario junto con la apariencia y funcionamiento de la página; la página de errores personalizada puede utilizar la misma página maestra y estilos como las otras páginas. La sección "Usar una página de Error personalizada" le guía a través de la creación de una página de error personalizado y configurarlo para mostrar si se produce una excepción no controlada. **Figura 3** ofrece un anticipo de esta página de error personalizado. Como puede ver, la apariencia y funcionamiento de la página de error es mucho más profesional que cualquiera de las pantallas de muerte de amarillo se muestra en las figuras 1 y 2.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Figura 3**: una página de Error personalizada ofrece una apariencia más adaptada  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image9.png))

Tómese un momento para inspeccionar la barra de direcciones del explorador de **figura 3**. Tenga en cuenta que la barra de direcciones muestra la dirección URL de la página de error personalizada (`/ErrorPages/Oops.aspx`). En las figuras 1 y 2 se muestran las pantallas de muerte amarillo en la misma página que se ha originado el error (`Genre.aspx`). La página de errores personalizados se pasa la dirección URL de la página donde se produjo el error a través de la `aspxerrorpath` parámetro de cadena de consulta.

## <a name="configuring-which-error-page-is-displayed"></a>Configuración de página de Error que se muestra

Cuál de las tres páginas de error posibles se muestra se basa en dos variables:

- La información de configuración en la `<customErrors>` sección, y
- Si el usuario está visitando el sitio local o remotamente.

El [ `<customErrors>` sección](https://msdn.microsoft.com/library/h0hfz6fc.aspx) en `Web.config` tiene dos atributos que afectan a la página de error se muestra: `defaultRedirect` y `mode`. El atributo `defaultRedirect` es opcional. Si se proporciona, especifica la dirección URL de la página de error personalizada e indica que se debe mostrar la página de error personalizado en lugar de la YSOD de Error en tiempo de ejecución. El `mode` atributo es obligatorio y acepta uno de estos tres valores: `On`, `Off`, o `RemoteOnly`. Estos valores tienen el siguiente comportamiento:

- `On`-indica que se muestra la página de error personalizada o el YSOD de Error en tiempo de ejecución a todos los visitantes, independientemente de si son locales o remotos.
- `Off`: Especifica que la YSOD de detalles de excepción se muestra a todos los visitantes, independientemente de si son locales o remotos.
- `RemoteOnly`-indica que la página de error personalizada o el YSOD de Error en tiempo de ejecución se muestra a los visitantes remotos, mientras que la YSOD de detalles de excepción se muestra a los visitantes del locales.

A menos que se especifique lo contrario, ASP.NET actúa como si hubiera establecido el atributo de modo `RemoteOnly` y no se ha especificado un `defaultRedirect` valor. En otras palabras, el comportamiento predeterminado es que la YSOD de detalles de excepción se muestra a los visitantes locales mientras el YSOD de Error en tiempo de ejecución se muestra a los visitantes remotos. Puede invalidar este comportamiento predeterminado mediante la adición de un `<customErrors>` sección en su aplicación web`Web.config file.`

## <a name="using-a-custom-error-page"></a>Uso de una página de Error personalizado

Cada aplicación web debe tener una página de error personalizado. Ofrece una alternativa más profesional para el YSOD de Error en tiempo de ejecución, es fácil crear y configurar la aplicación para que use la página de error personalizada tarda solo unos segundos. El primer paso es crear la página de error personalizado. He agregado una nueva carpeta a la aplicación de reseñas de libros denominada `ErrorPages` y se agrega al que una nueva página ASP.NET denominado `Oops.aspx`. Que la página usen la misma página maestra que el resto de las páginas en el sitio para que ésta hereda automáticamente la misma apariencia y funcionamiento.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Figura 4**: crear una página de errores personalizada

A continuación, dedique unos minutos a crear el contenido de la página de error. He creado una página de error personalizado en su lugar simple con un mensaje que indica que se produjo un error inesperado y hacer copia de un vínculo a la página principal del sitio.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Figura 5**: diseñar su página de errores personalizados  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image14.png))

En la página de error completada, configure la aplicación web para utilizar la página de error personalizado en lugar de la YSOD de Error en tiempo de ejecución. Esto se consigue especificando la dirección URL de la página de error en la `<customErrors>` la sección `defaultRedirect` atributo. Agregue el siguiente marcado para su aplicación `Web.config` archivo:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

El marcado anterior configura la aplicación para mostrar el YSOD de detalles de excepción para los usuarios que visitan localmente, al usar la página de error personalizada Oops.aspx para aquellos usuarios que visitan de forma remota. Para ver esto en acción, implementar el sitio Web en el entorno de producción y, a continuación, visite la página de Genre.aspx en el sitio activo con un valor de cadena de consulta no válida. Debería ver la página de error personalizada (hacen referencia a **figura 3**).

Para comprobar que solo se muestre la página de error personalizada a los usuarios remotos, visite la `Genre.aspx` página con una cadena de consulta no válido desde el entorno de desarrollo. Se debe seguir viendo la YSOD de detalles de excepción (hacen referencia a **figura 1**). El `RemoteOnly` valor garantiza que los usuarios que visitan el sitio en el entorno de producción ven la página de error personalizada mientras los desarrolladores que trabajan de forma local continuarán ver los detalles de la excepción.

## <a name="notifying-developers-and-logging-error-details"></a>Notificar a los desarrolladores y registrar los detalles del Error

Errores que se producen en el entorno de desarrollo causados por el desarrollador sentado en su equipo. Información de la excepción se muestra en el YSOD de detalles de excepción y sabe qué pasos que estaba realizando cuando se produjo el error. Pero cuando se produce un error en producción, el programador no tiene ningún conocimiento que se produjo un error a menos que el usuario final visitar el sitio toma el tiempo para notificar el error. Y, incluso, si el usuario sale de la forma para avisar al equipo de desarrollo que se produjo un error, sin conocer el tipo de excepción, el mensaje y el seguimiento de pila puede ser difícil de diagnosticar la causa del error, sin olvidar corregirlo.

Por estos motivos es fundamental que cualquier error en el entorno de producción se registra en un almacén persistente (como una base de datos) y que los desarrolladores se les avise de este error. La página de errores personalizada puede parecer un buen lugar para hacer este registro y notificación. Desafortunadamente, la página de error personalizado no tiene acceso a los detalles del error y, por tanto, no se puede usar para registrar esta información. Las buenas noticias son que hay varias maneras de interceptar los detalles del error y para iniciar la sesión, y los tutoriales siguientes tres explorar este tema con más detalle.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Usando las páginas de Error personalizado diferente para Estados de Error distinto de HTTP

Cuando una excepción producida por una página ASP.NET y no se controla, la excepción percolates hasta el tiempo de ejecución ASP.NET, que muestra la página de error configurado. Si una solicitud entra en el motor ASP.NET pero no se puede procesar por algún motivo, quizás el archivo solicitado no se encuentra o leer han deshabilitado los permisos para el archivo -, a continuación, el motor ASP.NET genera un `HttpException`. Esta excepción, como las excepciones producidas desde páginas ASP.NET, se propaga hasta el tiempo de ejecución, provocando que se muestre la página de error adecuado.

Lo que esto significa que para la aplicación web en producción es que si un usuario solicita una página que no se encuentra, a continuación, verá la página de error personalizado. **Figura 6** muestra un ejemplo de este tipo. Dado que la solicitud es para una página inexistente (`NoSuchPage.aspx`), una `HttpException` se produce y se muestra la página de error personalizada (tenga en cuenta la referencia a `NoSuchPage.aspx` en el `aspxerrorpath` parámetro querystring).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**Figura 6**: el tiempo de ejecución de ASP.NET, se muestra la página de Error configurados en respuesta a una solicitud no válida  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image17.png)) 

De forma predeterminada, todos los tipos de errores producen la misma página de error personalizada que se mostrará. Sin embargo, puede especificar una página de error personalizado diferente para un específico HTTP estado código mediante `<error>` los elementos secundarios dentro de la `<customErrors>` sección. Por ejemplo, para que tenga una página de error diferentes mostrada en el caso de una página de error, que tiene un código de estado HTTP de 404, no encontrada actualizar el `<customErrors>` sección debe incluir el siguiente marcado:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Con este cambio en su lugar, cada vez que un usuario que visita de forma remota solicita un recurso ASP.NET que no existe, se le redirigirá a la `404.aspx` página de error personalizada en lugar de `Oops.aspx`. Como **figura 7** muestra, la `404.aspx` página puede incluir un mensaje más específico de la página de error personalizada general.

> [!NOTE]
> Extraer del repositorio [404 páginas de Error, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obtener instrucciones sobre cómo crear páginas de error 404 efectivo.


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**Figura 7**: la página de Error 404 personalizada muestra un mensaje más concretas que`Oops.aspx`  
 ([Haga clic aquí para ver la imagen a tamaño completo](displaying-a-custom-error-page-vb/_static/image20.png)) 

Dado que sabe que la `404.aspx` página solo se alcanza cuando el usuario realiza una solicitud para una página que no se encontró, se puede mejorar esta página de error personalizada para incluir funcionalidades para ayudar al usuario a resolver este tipo de error específico. Por ejemplo, podría crear una tabla de base de datos que asigna conocida las direcciones de URL incorrectas a buen direcciones URL y, a continuación, tiene la `404.aspx` página de errores personalizados ejecutar una consulta en que la tabla y sugerir páginas que el usuario intenta alcanzar.

> [!NOTE]
> Solo se muestra la página de error personalizado cuando se realiza una solicitud a un recurso controlado por el motor ASP.NET. Como se explicó en la [principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](core-differences-between-iis-and-the-asp-net-development-server-vb.md) tutorial, el servidor web puede administrar algunas solicitudes de sí mismo. De forma predeterminada, IIS web server procesa las solicitudes de contenido estático, como imágenes y archivos HTML sin invocar el motor ASP.NET. Por lo tanto, si el usuario solicita un archivo de imagen inexistente obtendrán nuevo mensaje de error 404 predeterminado de IIS en lugar de ASP. Página de error configurados de NET.


## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación ASP.NET, se muestra al usuario uno de tres páginas de error: la excepción detalles amarillo pantalla de muerte; el Error en tiempo de ejecución amarillo pantalla de muerte; o una página de error personalizada. Se muestra la página de error depende de la aplicación `<customErrors>` configuración y si el usuario está visitando local o remotamente. El comportamiento predeterminado es mostrar la YSOD de detalles de excepción para los visitantes del locales y el YSOD de Error en tiempo de ejecución para los visitantes remotos.

Mientras el YSOD de Error en tiempo de ejecución oculta la información de error potencialmente confidenciales desde el usuario visita el sitio, se interrumpe de apariencia y funcionamiento de su sitio y hace que la aplicación tenga una apariencia erróneo. Un enfoque más adecuado consiste en utilizar una página de error personalizada, lo que supone crear y diseñar la página de error personalizada y especificar su dirección URL en el `<customErrors>` la sección `defaultRedirect` atributo. Incluso puede tener varias páginas de errores personalizados para diferentes Estados de error HTTP.

La página de error personalizado es el primer paso en una estrategia para un sitio Web de producción de control de errores completo. Avisa al desarrollador del error y los detalles de registro también son pasos importantes. Los siguientes tres tutoriales explorar técnicas para la notificación de errores y registro.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas de error, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Instrucciones de diseño de excepciones](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de Error descriptivos](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Controlar y generar excepciones](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Correctamente con páginas de errores personalizadas en ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Anterior](strategies-for-database-development-and-deployment-vb.md)
[Siguiente](processing-unhandled-exceptions-vb.md)
