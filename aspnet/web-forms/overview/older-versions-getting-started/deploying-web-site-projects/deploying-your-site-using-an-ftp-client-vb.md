---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Implementación del sitio mediante un cliente FTP (VB) | Documentos de Microsoft
author: rick-anderson
description: La manera más sencilla de implementar una aplicación de ASP.NET es copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. Este...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 90ae866d82c4dbfd5c3e209c3d397df42d162515
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30888167"
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Implementación del sitio mediante un cliente FTP (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> La manera más sencilla de implementar una aplicación de ASP.NET es copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. Este tutorial muestra cómo utilizar a un cliente FTP para obtener los archivos desde el escritorio, el proveedor de hospedaje web.


## <a name="introduction"></a>Introducción

El tutorial anterior se han presentado una aplicación web de ASP.NET de revisión de libro simple, que consta de una serie de páginas ASP.NET, una página maestra, una base personalizada `Page` de la clase, un número de las imágenes y hojas de estilos tres CSS. Ahora estamos preparados implementar esta aplicación en un proveedor de hospedaje web, momento en que la aplicación estará accesible para cualquier usuario con una conexión a Internet.


En nuestras conversaciones en la [ *determinar qué archivos deben implementarse* ](determining-what-files-need-to-be-deployed-vb.md) tutorial, sabemos qué archivos deben copiarse en el proveedor de hospedaje web. (Recuerde que los archivos se copian depende de si la aplicación es explícita o automáticamente compilada). Pero, ¿cómo se obtienen los archivos desde el entorno de desarrollo (nuestro equipo de escritorio) hasta el entorno de producción (el servidor web administrado por el proveedor de hospedaje web)? El [ **F** ile **T** transferir **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) es un protocolo utilizado frecuentemente para copiar archivos de un equipo a otro a través de una red. Otra opción es extensiones de servidor de FrontPage (FPSE). Este tutorial se centra en el uso de software de cliente FTP independiente para implementar los archivos necesarios del entorno de desarrollo al entorno de producción.

> [!NOTE]
> Visual Studio incluye herramientas para publicar sitios Web a través de FTP; en el siguiente tutorial se trata estas herramientas, así como un vistazo a las herramientas que usarlas.


Para copiar los archivos mediante FTP, necesitamos un *cliente FTP* en el entorno de desarrollo. Un cliente FTP es una aplicación que está diseñada para copiar archivos desde el equipo está instalado en un equipo que ejecuta un *servidor FTP*. (Si su proveedor de hospedaje web admite las transferencias de archivos a través de FTP, igual que la mayor parte, a continuación, hay un servidor FTP que se ejecutan en sus servidores web.) Hay un número de aplicaciones de cliente FTP disponibles. El explorador web puede incluso doble como un cliente FTP. Mi cliente FTP favoritos y lo que va a utilizar para este tutorial, es [FileZilla](http://filezilla-project.org/), un cliente FTP gratuito y de código abierto que está disponible para Windows, Linux y equipos Mac. Cualquier cliente FTP funcionará, sin embargo, por lo que puede usar cualquier cliente es más cómodo.

Si está siguiendo a lo largo del se necesita crear una cuenta con un proveedor de hospedaje web antes de puede completar este tutorial o siguientes. Como se indicó en el tutorial anterior, hay un conjunto de empresas de proveedor de host de web con una amplia variedad de precios, características y calidad de servicio. Para esta serie de tutoriales va a utilizar [descuento ASP.NET](http://discountasp.net) como mi host web proveedor, pero puede seguir junto con cualquier proveedor de hospedaje web siempre y cuando admite la versión ASP.NET se desarrolla su sitio en. (Estos tutoriales se crearon mediante ASP.NET 3.5). Además, dado que se va a copiar los archivos para el proveedor de hospedaje web mediante FTP en este tutorial y en el futuro las es imperativo que su proveedor de hospedaje web admite el acceso FTP a sus servidores web. Prácticamente todos los proveedores de host de web ofrecen esta característica, pero debe comprobar antes de registrarse.

## <a name="deploying-the-book-review-web-application-project"></a>Implementar el proyecto de aplicación Web de libro revisión

Recuerde que hay dos versiones de la aplicación web de revisión de libro: uno se implementan utilizando el modelo de proyecto de aplicación Web (BookReviewsWAP) y el otro con el modelo de proyecto de sitio Web (BookReviewsWSP). El tipo de proyecto afecta a si el sitio se compila automáticamente o de forma explícita, y ese modelo de compilación determina qué archivos se deben implementar. Por lo tanto, examinaremos implementar los proyectos BookReviewsWAP y BookReviewsWSP por separado, a partir de la BookReviewsWAP. Tómese un momento para descargar estas dos aplicaciones de ASP.NET si aún no lo ha hecho.

Iniciar el proyecto BookReviewsWAP si se desplaza a la `BookReviewsWAP` carpeta y haga doble clic en el `BookReviewsWAP.sln` archivo. Antes de implementar el proyecto es importante compilar para asegurarse de que los cambios en el código fuente se incluyen en el ensamblado compilado. Para compilar el proyecto vaya al menú de compilación y elija la opción de menú Generar BookReviewsWAP. Esto compila el código fuente en el proyecto en un único ensamblado, `BookReviewsWAP.dll`, que se coloca en el `Bin` carpeta.

Ahora estamos listos implementar los archivos necesarios. Inicie al cliente FTP y conéctese al servidor web a su proveedor de hospedaje web. (Cuando se registra con una empresa de hospedaje web enviará por correo electrónico, obtener información sobre cómo conectarse al servidor FTP; Esto incluye la dirección para el servidor FTP, así como un nombre de usuario y una contraseña).

Copie los siguientes archivos desde el escritorio a la carpeta del sitio Web raíz en el proveedor de hospedaje web. Cuando usted FTP en el servidor web en la web hospeda proveedor es probable que en el directorio raíz del sitio Web. Sin embargo, algunos proveedores de host web tienen una subcarpeta denominada `www` o `wwwroot` que actúa como la carpeta raíz para los archivos del sitio Web. Por último, cuando FTPing los archivos que necesite crear la estructura de carpeta correspondiente en el entorno de producción - la `Bin` carpeta, el `Fiction` carpeta, el `Images` carpeta y así sucesivamente.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- El contenido completo de la `Styles` carpeta
- El contenido completo de la `Images` carpeta (y sus subcarpetas, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

La figura 1 muestra FileZilla después de haberse copiados los archivos necesarios. FileZilla muestra los archivos en el equipo local a la izquierda y los archivos en el equipo remoto conectado a la derecha. Como se muestra en la figura 1, los archivos de código fuente ASP.NET, como `About.aspx.vb`, se encuentran en el equipo local (el entorno de desarrollo), pero no se copiaron en el proveedor de hospedaje web (el entorno de producción), porque no es necesario implementar al usar archivos de código compilación explícita.

> [!NOTE]
> No hay peligro en que tiene los archivos de código fuente en el servidor de producción, tal y como se omiten. ASP.NET prohíbe las solicitudes HTTP para archivos de código fuente de forma predeterminada, por lo que incluso si los archivos de código fuente están presentes en el servidor de producción son inaccesibles a los visitantes a su sitio Web. (Es decir, si un usuario intenta visitar `http://www.yoursite.com/Default.aspx.vb` obtendrán una página de error que explica que estos tipos de archivos - `.vb` archivos - están prohibidas.)


[![Utilice a un cliente FTP para copiar los archivos necesarios desde el escritorio en el servidor Web en el proveedor de hospedaje Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Figura 1**: utilizar un cliente de FTP para copiar los archivos necesarios desde el escritorio en el servidor Web en el proveedor de hospedaje Web ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Después de implementar su sitio Tómese un momento para probar el sitio. Si ha adquirido un nombre de dominio y la configuración de DNS correctamente, puede visitar el sitio especificando su nombre de dominio. Como alternativa, el proveedor de hospedaje web debe ha proporcionado, con una dirección URL para el sitio, que tendrá un aspecto parecido *accountname*. *webhostprovider*.com o *webhostprovider*.com /*accountname*. Por ejemplo, la dirección URL de mi cuenta de ASP.NET de descuento es: `http://httpruntime.web703.discountasp.net`.

La figura 2 muestra el sitio de reseñas de libros implementado. Tenga en cuenta que puedo ver en ASP descuento. Servidores de red, en `http://httpruntime.web703.discountasp.net`. En este momento cualquiera que tenga una conexión a Internet puede ver mi sitio Web. Tal como se podría esperar, el sitio parece y se comporta igual que cuando se prueba en el entorno de desarrollo.

> [!NOTE]
> Si se produce un error cuando la aplicación de visualización Tómese un momento para asegurarse de que implementa el conjunto correcto de archivos. A continuación, compruebe el mensaje de error para ver si revela ninguna pista sobre el problema. A continuación, puede optar por el departamento de soporte técnico de su compañía de host web o publique su pregunta en el foro adecuado en el [foros de ASP.NET](https://forums.asp.net/).


[![El sitio de las revisiones del libro es ahora accesible para cualquiera que tenga una conexión a Internet.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Figura 2**: el sitio de las revisiones del libro es ahora accesible para cualquiera que tenga una conexión a Internet ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Implementar el proyecto de sitio Web de revisión de libro

Al implementar una aplicación ASP.NET que utiliza la compilación automática, por ejemplo, el proyecto de sitio Web BookReviewsWSP, hay un ensamblado compilado en el `Bin` carpeta. Como resultado, los archivos de código fuente de la aplicación web deben implementarse en el entorno de producción. Analicemos este proceso.

Igual que con el proyecto de aplicación Web es conveniente generar primero la aplicación antes de implementarla. Al compilar un proyecto de sitio Web no crea un ensamblado, busque los errores de tiempo de compilación en la página. Mejor para buscar estos errores ahora en lugar de tener un visitante a su sitio detectarlos automáticamente.

Una vez que haya generado correctamente el proyecto, utilice al cliente FTP para copiar los siguientes archivos en la carpeta del sitio Web raíz en el proveedor de hospedaje web. Debe crear la estructura de carpeta correspondiente en el entorno de producción.

> [!NOTE]
> Si ya ha implementado el BookReviewsWAP proyecto pero todavía desea intentar implementar el proyecto BookReviewsWSP, primero elimine todos los archivos en el servidor web que se han cargado al implementar BookReviewsWAP y, a continuación, implementar los archivos para BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- El contenido completo de la `Styles` carpeta
- El contenido completo de la `Images` carpeta (y sus subcarpetas, `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

La figura 3 muestra FileZilla después de copiar los archivos necesarios. Como puede ver, ASP.NET archivos de código fuente, como `About.aspx.vb`, están presentes en el equipo local (el entorno de desarrollo) y el proveedor de hospedaje web (el entorno de producción) porque los archivos de código deben implementarse al usar automática compilación.


[![Utilizar a un cliente de FTP para copiar los archivos necesarios desde el escritorio en el servidor Web en el proveedor de hospedaje Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Figura 3**: utilizar un cliente de FTP para copiar los archivos necesarios desde el escritorio en el servidor Web en el proveedor de hospedaje Web ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


Modelo de compilación de la aplicación no afecta a la experiencia del usuario. Las mismas páginas ASP.NET son accesibles y su apariencia y el mismo comportamiento si se creó el sitio Web mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web.

## <a name="updating-a-web-application-on-production"></a>Actualizar una aplicación Web en producción

Implementación y desarrollo de aplicaciones web no son un proceso único. Por ejemplo, al crear el sitio Web de la reseña de libro generan las distintas páginas y escribió el código que lo acompañan en Mi PC (el entorno de desarrollo). Tras alcanzar un cierto estado estable, había implementado mi aplicación para que otros pueden visitar el sitio y leer mis opiniones. Pero la implementación no marcar el final de mi desarrollo en este sitio. ¿Agregar más reseñas de libros o implementa las características nuevas, como permitir que a mi visitantes a los libros de velocidad o escribir sus propios comentarios. Estas mejoras se desarrolló en el entorno de desarrollo y, cuando haya completado, se deben implementarse. Desarrollo e implementación, por lo tanto, son cíclicos. Desarrollar una aplicación y, a continuación, implementarlo. Mientras el sitio está activo y en producción, se agregan nuevas características y se han corregido errores con el tiempo, lo que necesita volver a implementar la aplicación. Y así sucesivamente, y así sucesivamente.

Como cabría esperar, al volver a implementar una aplicación web que solo debe copiar archivos nuevos y modificados. No es necesario para volver a implementar páginas sin cambios o servidor o cliente admiten archivos (aunque no hay peligro de hacerlo).

> [!NOTE]
> Hay que tener en cuenta al utilizar la compilación explícita es que cada vez que agregue una nueva página ASP.NET para el proyecto o realizar cambios relacionados con el código, debe volver a generar el proyecto, que actualiza el ensamblado en el `Bin` carpeta. Por lo tanto, debe copiar este ensamblado actualizado en producción al actualizar una aplicación web en producción (junto con otro contenido nuevo y actualizado).


También comprender que los cambios a la `Web.config` o los archivos en el `Bin` directory se detiene y reinicia el grupo de aplicaciones del sitio Web. Si el estado de sesión se almacena utilizando el `InProc` modo (valor predeterminado), a continuación, los visitantes de su sitio perderá su estado de sesión cada vez que se modifican estos archivos de clave. Para evitar este problema, considere la posibilidad de almacenar la sesión mediante la `StateServer` o `SQLServer` modos. Para obtener más información acerca de este tema leer [Session-State Modes](https://msdn.microsoft.com/library/ms178586.aspx).

Por último, tenga en cuenta que volver a implementar una aplicación puede tardar desde unos segundos a varios minutos, dependiendo del número y tamaño de los archivos que deben copiarse en el entorno de producción. Durante este período, los usuarios que visitan su sitio pueden experimentar errores o un comportamiento extraño. Puede "desactivar" toda la aplicación mediante la adición de una página denominada `App_Offline.htm` al directorio raíz de la aplicación que se explica a los usuarios que el sitio está fuera de servicio de mantenimiento (o cualquier otra) y puede realizar copias de seguridad en breve. Cuando el `App_Offline.htm` archivo está presente, el tiempo de ejecución ASP.NET redirige todas las solicitudes entrantes a esa página.

## <a name="summary"></a>Resumen

Implementar una aplicación web implica copiar los archivos necesarios del entorno de desarrollo al entorno de producción. La forma más común por el que se transfieren archivos a través de una red es el protocolo de transferencia de archivos (FTP) y la mayoría de los proveedores de host de web admiten el acceso FTP a sus servidores web. En este tutorial, hemos visto cómo utilizar a un cliente FTP para implementar los archivos necesarios en el servidor web. Una vez implementado, el sitio Web se pueden visitar cualquier usuario con una conexión a Internet.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Aplicación\_Offline.htm y trabajar con la característica de "Errores compatible de Internet Explorer"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado de sesión](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Anterior](determining-what-files-need-to-be-deployed-vb.md)
> [Siguiente](deploying-your-site-using-visual-studio-vb.md)
