---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: Mostrar una página de Error personalizada (VB) | Microsoft Docs
author: rick-anderson
description: ¿Lo que ve el usuario cuando se produce un error en tiempo de ejecución en una aplicación web ASP.NET? La respuesta depende de la del sitio Web &lt;customErrors&gt; configuración...
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 749b754903fb8e164b77edff4216fb2e2cf057b6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812930"
---
<a name="displaying-a-custom-error-page-vb"></a>Mostrar una página de Error personalizada (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) o [descargar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> ¿Lo que ve el usuario cuando se produce un error en tiempo de ejecución en una aplicación web ASP.NET? La respuesta depende de la del sitio Web &lt;customErrors&gt; configuración. De forma predeterminada, los usuarios ven una pantalla amarilla cadenas tan antiestética proclamando que se ha producido un error en tiempo de ejecución. Este tutorial muestra cómo personalizar esta configuración a la página de error personalizada para mostrar un estéticamente agradables que coincida con la apariencia de su sitio.


## <a name="introduction"></a>Introducción

En un mundo perfecto no habría ningún error de tiempo de ejecución. Los programadores escribir código demasiado un error y con la validación de entrada de usuario sólidas y externos recursos, como servidores de base de datos y servidores de correo electrónico nunca se queden sin conexión. Por supuesto, en realidad, los errores son inevitables. Las clases de .NET Framework indican un error iniciando una excepción. Por ejemplo, una llamada a un objeto SqlConnection método Open del objeto establece una conexión a la base de datos especificada por una cadena de conexión. Sin embargo, si la base de datos está inactiva o si las credenciales en la cadena de conexión son válidas, a continuación, el método Open produce un `SqlException`. Las excepciones pueden controlarse mediante el uso de `Try/Catch/Finally` bloques. Si el código dentro de un `Try` bloque produce una excepción, el control se transfiere al bloque catch adecuado donde el desarrollador puede intentar recuperarse del error. Si no hay ningún bloque catch coincidente, o si el código que produjo la excepción no está en un bloque try, la excepción percolates hasta la pila de llamadas de search `Try/Catch/Finally` bloques.

Si la excepción se propaga hasta el tiempo de ejecución ASP.NET sin que se controla, el [ `HttpApplication` clase](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)del [ `Error` eventos](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) se genera y configurado *página de error*  se muestra. De forma predeterminada, ASP.NET muestra una página de error afectuosamente se conoce como el [pantalla amarilla de la muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Hay dos versiones de la YSOD: muestra los detalles de la excepción, un seguimiento de pila y otra información útil para los desarrolladores depurar la aplicación (consulte **figura 1**); la otra sólo indica que se ha producido un error en tiempo de ejecución (consulte  **Figura 2**).

Los detalles de excepción YSOD es bastante útil para los desarrolladores depurar la aplicación, pero que se muestra un YSOD a los usuarios finales es mal gusto e y poco profesional. En su lugar, debe tener los usuarios finales a una página de error que mantiene la apariencia y comportamiento de la carpeta del sitio con prose más fácil de usar que describe la situación. La buena noticia es que es bastante fácil crear una página de error personalizado. Este tutorial comienza con una mirada a ASP. Páginas de error diferentes de la red. A continuación, se muestra cómo configurar la aplicación web para mostrar a los usuarios una página de error personalizado en el caso de un error.

### <a name="examining-the-three-types-of-error-pages"></a>Examen de los tres tipos de páginas de Error

Se muestra cuando se produce una excepción no controlada en una aplicación ASP.NET de tres tipos de páginas de error:

- La página de error de excepción detalles de la pantalla amarilla de muerte,
- La página de error en tiempo de ejecución Error pantalla amarilla de muerte, o
- Una página de error personalizado

Los desarrolladores de la página de error más familiarizado con son el YSOD de detalles de excepción. De forma predeterminada, esta página se muestra a los usuarios que visitan localmente y, por tanto, es la página que ve cuando se produce un error al probar el sitio en el entorno de desarrollo. Como su nombre implica, el YSOD de detalles de excepción proporciona detalles sobre la excepción: el tipo, el mensaje y el seguimiento de pila. Además, si se produjo la excepción mediante código de clase de código subyacente de la página ASP.NET y si la aplicación está configurada para la depuración el YSOD detalles de excepción también mostrará esta línea de código (y unas pocas líneas de código anterior y por debajo de él).

**Figura 1** muestra la página YSOD detalles de excepción. Tenga en cuenta la dirección URL en la ventana del explorador dirección: `http://localhost:62275/Genre.aspx?ID=foo`. Recuerde que la `Genre.aspx` página enumera las reseñas de libros de un género concreto. Requiere que `GenreId` valor (una `uniqueidentifier`) pasará a través de la cadena de consulta; por ejemplo, es la dirección URL apropiada para ver las revisiones de ficción `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Si no`uniqueidentifier` se pasa el valor de a través de la cadena de consulta (por ejemplo, "foo") se produce una excepción.

> [!NOTE]
> Para reproducir este error en la aplicación de demostración web disponible para su descarga, puede visitar `Genre.aspx?ID=foo` directamente o haga clic en el vínculo "Generar un Error en tiempo de ejecución" `Default.aspx`.


Tenga en cuenta la información de excepción que se presentan en **figura 1**. El mensaje de excepción, "Error de conversión al convertir de una cadena de caracteres a uniqueidentifier" está presente en la parte superior de la página. El tipo de la excepción, `System.Data.SqlClient.SqlException`, se muestra, también. También es el seguimiento de pila.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Figura 1**: los detalles de excepción YSOD incluye información sobre la excepción  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image3.png))

El otro tipo de YSOD es el YSOD de Error en tiempo de ejecución y se muestra en **figura 2**. El visitante que se ha producido un error en tiempo de ejecución le informa de la YSOD de Error en tiempo de ejecución, pero no incluye toda la información sobre la excepción que se produjo. (Sin embargo, lo, proporcionan instrucciones sobre cómo hacer que puede ver los detalles del error modificando la `Web.config` archivo, que forma parte de lo que hace que dicha un YSOD parezca poco profesional.)

De forma predeterminada, se muestra el YSOD de Error en tiempo de ejecución a los usuarios que visitan de forma remota (a través de http://www.yoursite.com), tal como se evidencia por la dirección URL en la barra de direcciones del explorador de **figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Las dos pantallas YSOD diferentes existen porque los desarrolladores están interesados en saber los detalles del error, pero no se debe mostrar dicha información en un sitio activo tal como puede revelar posibles vulnerabilidades de seguridad u otra información confidencial a cualquier persona que visite el sitio.

> [!NOTE]
> Si está siguiendo y usa DiscountASP.NET como host de web, es posible que tenga en cuenta que el YSOD de Error en tiempo de ejecución no se muestran cuando se visita el sitio activo. Esto es porque DiscountASP.NET tiene sus servidores configurados para mostrar el YSOD detalles de excepción de forma predeterminada. La buena noticia es que puede invalidar este comportamiento predeterminado mediante la adición de un `<customErrors>` sección a su `Web.config` archivo. La sección de "configuración que Error es mostrar la página" examina el `<customErrors>` sección detalladamente.


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Figura 2**: el Error en tiempo de ejecución YSOD no incluye los detalles del Error  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image6.png))

El tercer tipo de página de error es la página de error personalizado, que es una página web que cree. La ventaja de una página de error personalizada es que tienen control completo sobre la información que se muestra al usuario junto con la apariencia y comportamiento de la página; página de error personalizada puede usar la misma página maestra y estilos como las otras páginas. La sección "Usar una página de Error personalizada" guiará a través de la creación de una página de error personalizado y configurarlo para mostrar si se produce una excepción no controlada. **Figura 3** ofrece un anticipo de esta página de error personalizado. Como puede ver, la apariencia de la página de error es mucho más profesional a cualquiera de las pantallas de muerte de amarillo se muestra en las figuras 1 y 2.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Figura 3**: una página de Error personalizados ofrece una apariencia más personalizada  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image9.png))

Dedique un momento para inspeccionar la barra de direcciones del explorador de **figura 3**. Tenga en cuenta que la barra de direcciones muestra la dirección URL de la página de error personalizado (`/ErrorPages/Oops.aspx`). En las figuras 1 y 2 se muestran las pantallas de muerte amarillo en la misma página que se originó el error (`Genre.aspx`). Página de error personalizada se pasa la dirección URL de la página donde se produjo el error a través de la `aspxerrorpath` parámetro querystring.

## <a name="configuring-which-error-page-is-displayed"></a>Configuración de página de Error que se muestra.

Se muestra cuál de las tres páginas de error posibles se basa en dos variables:

- La información de configuración en el `<customErrors>` sección, y
- Si el usuario está visitando el sitio local o remotamente.

El [ `<customErrors>` sección](https://msdn.microsoft.com/library/h0hfz6fc.aspx) en `Web.config` tiene dos atributos que afectan a la página de error que se muestra: `defaultRedirect` y `mode`. El atributo `defaultRedirect` es opcional. Si se proporciona, especifica la dirección URL de la página de error personalizado e indica que debe mostrarse la página de error personalizado en lugar de la YSOD de Error en tiempo de ejecución. El `mode` atributo es obligatorio y acepta uno de estos tres valores: `On`, `Off`, o `RemoteOnly`. Estos valores tienen el siguiente comportamiento:

- `On` -indica que se muestra la página de error personalizado o el YSOD de Error en tiempo de ejecución a todos los visitantes, independientemente de si son locales o remotos.
- `Off` : Especifica que el YSOD de detalles de excepción se muestra a todos los visitantes, independientemente de si son locales o remotos.
- `RemoteOnly` -indica que los visitantes de remoto, se muestra la página de error personalizado o el YSOD de Error en tiempo de ejecución mientras el YSOD detalles de excepción se muestra a los visitantes locales.

A menos que se especifique lo contrario, ASP.NET actúa como si hubiera establecido el atributo de modo `RemoteOnly` y no se ha especificado un `defaultRedirect` valor. En otras palabras, el comportamiento predeterminado es que la YSOD detalles de excepción se muestra a los visitantes locales mientras la YSOD de Error en tiempo de ejecución se muestra a los visitantes remotos. Puede invalidar este comportamiento predeterminado mediante la adición de un `<customErrors>` sección a la aplicación web `Web.config file.`

## <a name="using-a-custom-error-page"></a>Uso de una página de Error personalizado

Cada aplicación web debe tener una página de error personalizado. Proporciona una alternativa más profesional a la YSOD de Error en tiempo de ejecución, es fácil crear y configurar la aplicación para que use la página de error personalizado tarda solo unos momentos. El primer paso es crear una página de error personalizada. He agregado una nueva carpeta a la aplicación de reseñas de libros denominada `ErrorPages` y se agrega al que una página ASP.NET nueva denominada `Oops.aspx`. Tiene la página utilizan la misma página maestra que el resto de las páginas en el sitio para que herede automáticamente la misma apariencia y comportamiento.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Figura 4**: crear una página de Error personalizado

A continuación, dedique unos minutos crear el contenido de la página de error. He creado una página de error personalizada bastante sencilla con un mensaje que indica que se produjo un error inesperado y realizar una copia de un vínculo a la página principal de la carpeta del sitio.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Figura 5**: diseñar la página de Error personalizado  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image14.png))

En la página de error completada, configure la aplicación web para usar la página de error personalizado en lugar de la YSOD de Error en tiempo de ejecución. Esto se consigue especificando la dirección URL de la página de error en la `<customErrors>` la sección `defaultRedirect` atributo. Agregue el siguiente marcado para su aplicación `Web.config` archivo:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

El marcado anterior configura la aplicación para mostrar el YSOD detalles de excepción para los usuarios que visitan localmente, al usar la página de error personalizada Oops.aspx para aquellos usuarios que visitan de forma remota. Para ver esto en acción, implemente su sitio Web en el entorno de producción y, a continuación, visite la página de Genre.aspx en el sitio activo con un valor de cadena de consulta no válido. Debería ver la página de error personalizada (vuelva a consultar **figura 3**).

Para comprobar que la página de error personalizada solo se muestra a los usuarios remotos, visite la `Genre.aspx` página con una cadena de consulta no válida desde el entorno de desarrollo. Todavía debe ver el YSOD de detalles de excepción (vuelva a consultar **figura 1**). El `RemoteOnly` configuración garantiza que los usuarios que visitan el sitio en el entorno de producción ven la página de error personalizado mientras siguen los desarrolladores que trabajan de forma local ver los detalles de la excepción.

## <a name="notifying-developers-and-logging-error-details"></a>Notificar a los desarrolladores y los detalles del Error de registro

Errores que ocurren en el entorno de desarrollo causados por el desarrollador sentado en su equipo. Información de la excepción se muestra en el YSOD de detalles de excepción y sabe qué pasos que estaba realizando cuando se produjo el error. Pero cuando se produce un error en producción, el desarrollador no tiene conocimiento que se produjo un error a menos que el usuario final, visite el sitio toma el tiempo para notificar el error. Y aunque el usuario sale del dirigía a alertar al equipo de desarrollo que se produjo un error, sin conocer el tipo de excepción, el mensaje y el seguimiento de la pila puede ser difícil de diagnosticar la causa del error, hablar de solucionarlo.

Por estos motivos es primordial que algún error en el entorno de producción se registra en algún almacén persistente (por ejemplo, una base de datos) y que los desarrolladores se le avisará de este error. Página de error personalizada puede parecer un buen lugar para hacer esto, registro y notificación. Lamentablemente, la página de error personalizado no tiene acceso a los detalles del error y, por tanto, no se puede usar para registrar esta información. La buena noticia es que hay varias maneras de interceptar los detalles del error y para iniciar la sesión, y los siguientes tres tutoriales exploran en este tema con más detalle.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Uso de páginas de errores personalizados distintos para Estados de Error HTTP diferente

Cuando una excepción producida por una página ASP.NET y no se controla, la excepción percolates hasta el tiempo de ejecución ASP.NET, que muestra la página de error configurado. Si una solicitud entra en el motor de ASP.NET, pero no se puede procesar por algún motivo, quizás el archivo solicitado no se encuentra o leer los permisos se han deshabilitado para el archivo -, a continuación, el motor ASP.NET genera un `HttpException`. Esta excepción, como las excepciones producidas desde páginas ASP.NET, se propaga hasta el tiempo de ejecución, provocando que se muestre la página de error adecuado.

Esto significa para la aplicación web en producción es que si un usuario solicita una página que no se encuentra a continuación, verá la página de error personalizado. **Figura 6** muestra un ejemplo. Dado que la solicitud es para una página inexistente (`NoSuchPage.aspx`), un `HttpException` se produce y se muestra la página de error personalizada (tenga en cuenta la referencia a `NoSuchPage.aspx` en el `aspxerrorpath` parámetro querystring).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**Figura 6**: el tiempo de ejecución de ASP.NET se muestra la página de Error configurada en respuesta a una solicitud no válida  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image17.png)) 

De forma predeterminada, todos los tipos de error producen la misma página de error personalizado que se mostrará. Sin embargo, puede especificar una página de error personalizado diferente para un específico HTTP estado código con `<error>` los elementos secundarios dentro de la `<customErrors>` sección. Por ejemplo, para que tenga una página de error diferentes mostrada en el caso de una página no se encuentra, que tiene un código de estado HTTP de 404, actualice el `<customErrors>` sección debe incluir el marcado siguiente:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Con este cambio en su lugar, cada vez que un usuario que visita de forma remota solicita un recurso ASP.NET que no existe, se le redirigirá a la `404.aspx` página de error personalizada en lugar de `Oops.aspx`. Como **figura 7** , se ilustra la `404.aspx` página puede incluir un mensaje más específico que la página de error personalizado general.

> [!NOTE]
> Desproteger [404 páginas de Error, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obtener instrucciones sobre cómo crear eficaces páginas de error 404.


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**Figura 7**: la página de Error 404 personalizado muestra un mensaje más orientado a `Oops.aspx`  
 ([Haga clic aquí para ver imagen en tamaño completo](displaying-a-custom-error-page-vb/_static/image20.png)) 

Dado que sabe que la `404.aspx` página solo se alcanza cuando el usuario realiza una solicitud para una página que no se encontró, puede mejorar esta página de error personalizada para incluir la funcionalidad para ayudar al usuario a abordar este tipo de error específico. Por ejemplo, podría crear una tabla de base de datos que se asigna conocida direcciones URL incorrectas a direcciones URL buena y, a continuación, tiene la `404.aspx` página de error personalizada que ejecute una consulta en que la tabla y sugerir las páginas que el usuario intenta alcanzar.

> [!NOTE]
> Solo se muestra la página de error personalizado cuando se realiza una solicitud a un recurso controlado por el motor de ASP.NET. Como se explicó en la [diferencias principales entre IIS y el servidor de desarrollo de ASP.NET](core-differences-between-iis-and-the-asp-net-development-server-vb.md) tutorial, el servidor web puede controlar determinadas solicitudes de sí mismo. De forma predeterminada, IIS web server procesa las solicitudes de contenido estático como imágenes y archivos HTML sin invocar el motor de ASP.NET. Por lo tanto, si el usuario solicita un archivo de imagen inexistente obtendrá nuevo mensaje de error 404 predeterminado de IIS en lugar de ASP. Página de error configurada de la red.


## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación ASP.NET, se muestra al usuario uno de tres páginas de error: la excepción detalles amarillo pantalla de la muerte; el Error en tiempo de ejecución de color amarillo pantalla de la muerte; o una página de error personalizada. Página de error que se muestra depende de la aplicación `<customErrors>` configuración y si el usuario está visitando local o remotamente. El comportamiento predeterminado es mostrar la YSOD detalles de excepción para los visitantes locales y el YSOD de Error en tiempo de ejecución a los visitantes remotos.

Mientras el YSOD de Error en tiempo de ejecución oculta la información de error potencialmente confidenciales desde el usuario visita el sitio, se interrumpe en la apariencia de su sitio y hace que su aplicación tiene errores de aspecto. Un mejor enfoque es usar una página de error personalizada, lo que implica crear y diseñar la página de error personalizado y especificar su dirección URL en el `<customErrors>` la sección `defaultRedirect` atributo. Incluso puede tener varias páginas de error personalizado para diversos estados de error HTTP.

Página de error personalizada es el primer paso en una estrategia para un sitio Web de producción de control de errores completa. Avisa al desarrollador del error y sus detalles de registro también son pasos importantes. Los tres tutoriales siguientes exploran las técnicas para la notificación de error y el registro.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas de error, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Instrucciones de diseño de excepciones](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de Error descriptivo](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Controlar y generar excepciones](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Usar correctamente las páginas de Error personalizado en ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Anterior](strategies-for-database-development-and-deployment-vb.md)
> [Siguiente](processing-unhandled-exceptions-vb.md)
