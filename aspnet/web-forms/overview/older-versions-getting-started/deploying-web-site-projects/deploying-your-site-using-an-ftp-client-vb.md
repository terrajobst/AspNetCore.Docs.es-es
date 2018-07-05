---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Implementar el sitio mediante un cliente FTP (VB) | Microsoft Docs
author: rick-anderson
description: La manera más sencilla de implementar una aplicación ASP.NET es copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. Thi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: e886e07309654f34c7890f1e3ac79c7732a2e015
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367766"
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Implementar el sitio mediante un cliente FTP (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) o [descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> La manera más sencilla de implementar una aplicación ASP.NET es copiar manualmente los archivos necesarios del entorno de desarrollo en el entorno de producción. Este tutorial muestra cómo usar a un cliente FTP para obtener los archivos desde el escritorio, el proveedor de hospedaje web.


## <a name="introduction"></a>Introducción

El tutorial anterior introdujo una aplicación web de ASP.NET de revisión del libro simple, que consta de un puñado de páginas ASP.NET, una página maestra, una base personalizada `Page` clase, un número de imágenes y hojas de estilos tres CSS. Ahora estamos preparados para implementar esta aplicación en un proveedor de hospedaje web, momento en que la aplicación será accesible a cualquier persona con una conexión a Internet!


En nuestras conversaciones en el [ *determinar qué archivos se deben implementarse* ](determining-what-files-need-to-be-deployed-vb.md) tutorial, sabemos qué archivos se deben copiarse en el proveedor de hospedaje web. (Recuerde que qué archivos se copian depende de si la aplicación es explícita o automáticamente compilada). Pero, ¿cómo se obtienen los archivos del entorno de desarrollo (nuestro equipo de escritorio) hasta el entorno de producción (el servidor web administrado por el proveedor de hospedaje web)? El [ **F** ile **T** transferir **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) es un protocolo utilizado frecuentemente para copiar archivos desde un equipo a otro a través de una red. Otra opción es FrontPage Server Extensions (FPSE). En este tutorial se centra en usar el software de cliente FTP independiente para implementar los archivos necesarios del entorno de desarrollo para el entorno de producción.

> [!NOTE]
> Visual Studio incluye herramientas para publicar sitios Web a través de FTP; Estas herramientas, así como un vistazo a las herramientas que usan FPSE, se trata en el siguiente tutorial.


Para copiar los archivos mediante FTP, necesitamos un *cliente FTP* en el entorno de desarrollo. Un cliente FTP es una aplicación que está diseñada para copiar archivos desde el equipo está instalado en un equipo que ejecuta un *servidor FTP*. (Si su proveedor de hospedaje web admite las transferencias de archivos a través de FTP, como hace la mayor parte, a continuación, hay un servidor FTP que se ejecutan en sus servidores web.) Hay un número de aplicaciones cliente FTP. El explorador web puede incluso doble como un cliente FTP. Mi cliente FTP favoritos y el va a utilizar para este tutorial, es [FileZilla](http://filezilla-project.org/), un cliente FTP gratuito, código abierto que está disponible para Windows, Linux y Mac. Cualquier cliente FTP funcionará, sin embargo, por lo que puede usar cualquier cliente se sienta más cómodo.

Si está siguiendo, se necesita crear una cuenta con un proveedor de hospedaje web antes de puede completar este tutorial o siguientes. Como se indicó en el tutorial anterior, hay un conjunto de empresas de proveedor de host de web con un amplio espectro de calidad de servicio, características y precios. Para esta serie de tutoriales va a utilizar [descuento ASP.NET](http://discountasp.net) como mi host web proveedor, pero puede seguir junto con cualquier proveedor de hospedaje web, siempre que admiten el sitio se desarrolla en la versión de ASP.NET. (Estos tutoriales se han creado con ASP.NET 3.5). Además, dado que se va a copiar los archivos del proveedor de hospedaje web mediante FTP en este tutorial y en el futuro aquellas es imperativo que su proveedor de hospedaje web admite el acceso FTP a sus servidores web. Prácticamente todos los proveedores de host de web ofrecen esta característica, pero debe comprobar antes de registrarse.

## <a name="deploying-the-book-review-web-application-project"></a>Implementar el proyecto de aplicación Web de libro revisión

Recuerde que existen dos versiones de la aplicación web de revisión del libro: uno implementado mediante el modelo de proyecto de aplicación Web (BookReviewsWAP) y otra con el modelo de proyecto de sitio Web (BookReviewsWSP). El tipo de proyecto influye en si el sitio se compila automáticamente o explícitamente, y ese modelo de compilación determina qué archivos se deben implementarse. Por lo tanto, examinaremos la implementación de proyectos BookReviewsWAP y BookReviewsWSP por separado, empezando por el BookReviewsWAP. Dedique un momento para descargar estas dos aplicaciones de ASP.NET si aún no lo ha hecho.

Iniciar el proyecto BookReviewsWAP, vaya a la `BookReviewsWAP` carpeta y haga doble clic en el `BookReviewsWAP.sln` archivo. Antes de implementar el proyecto es importante compílela para asegurarse de que los cambios en el código fuente se incluyen en el ensamblado compilado. Para compilar el proyecto, en el menú de la compilación y elija la opción de menú Generar BookReviewsWAP. Esto compila el código fuente en el proyecto en un único ensamblado, `BookReviewsWAP.dll`, que se coloca en el `Bin` carpeta.

Ahora estamos listos para implementar los archivos necesarios. Inicie al cliente de FTP y conéctese al servidor web en su proveedor de hospedaje web. (Al registrarse con una empresa de hospedaje web enviará por correo electrónico, información sobre cómo conectarse al servidor FTP; Esto incluye la dirección para el servidor FTP, así como un nombre de usuario y contraseña).

Copie los siguientes archivos desde el escritorio a la carpeta del sitio Web raíz en su proveedor de hospedaje web. Cuando usted FTP en el servidor web en la web hospeda el proveedor es probable que en el directorio raíz del sitio Web. Sin embargo, algunos proveedores de host de web tienen una subcarpeta denominada `www` o `wwwroot` que actúa como la carpeta raíz para los archivos del sitio Web. Por último, cuando FTPing los archivos es posible que necesita crear la estructura de carpetas correspondientes en el entorno de producción: la `Bin` carpeta, el `Fiction` carpeta, el `Images` carpeta y así sucesivamente.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Todo el contenido de la `Styles` carpeta
- Todo el contenido de la `Images` carpeta (y sus subcarpetas, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Figura 1 muestra FileZilla, una vez copiados los archivos necesarios. FileZilla muestra los archivos en el equipo local a la izquierda y los archivos en el equipo remoto conectado a la derecha. Como se muestra en la figura 1, los archivos de código fuente ASP.NET, como `About.aspx.vb`, son en el equipo local (el entorno de desarrollo), pero no se copiaron en el proveedor de hospedaje web (el entorno de producción) porque no deben implementarse al usar archivos de código compilación explícita.

> [!NOTE]
> No hay ningún riesgo de tener los archivos de código fuente en el servidor de producción, tal como se pasan por alto. ASP.NET prohíbe las solicitudes HTTP para archivos de código fuente de forma predeterminada para que incluso si los archivos de código fuente están presentes en el servidor de producción son inaccesibles para los visitantes de su sitio. (Es decir, si un usuario intenta visitar `http://www.yoursite.com/Default.aspx.vb` obtendrán una página de error que explica que estos tipos de archivos - `.vb` archivos - están prohibidas.)


[![Usar a un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hospedaje Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Figura 1**: uso de un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hospedaje Web ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Después de implementar su sitio, dedique un momento para probar el sitio. Si ha adquirido un nombre de dominio y configurar el DNS correctamente, puede visitar su sitio escribiendo su nombre de dominio. Como alternativa, el proveedor de hospedaje web debe ha proporcionado, con una dirección URL del sitio, que tendrá un aspecto parecido *accountname*. *webhostprovider*.com o *webhostprovider*.com /*accountname*. Por ejemplo, es la dirección URL de mi cuenta de ASP.NET de descuento: `http://httpruntime.web703.discountasp.net`.

Figura 2 muestra el sitio implementado de reseñas de libros. Tenga en cuenta que estoy viendo en ASP de descuento. Los servidores de la red, en `http://httpruntime.web703.discountasp.net`. En este momento, cualquier persona con una conexión a Internet podría ver mi sitio Web! Como se podría esperar, el sitio parece y se comporta igual que cuando se prueba en el entorno de desarrollo.

> [!NOTE]
> Si recibe un error al visualizar la aplicación dedique un momento para asegurarse de que ha implementado el conjunto correcto de archivos. A continuación, compruebe el mensaje de error para ver si revelan las pistas sobre el problema. A continuación, puede activar al departamento de soporte técnico de su compañía de host de web o publicar su pregunta en el foro adecuado en el [foros de ASP.NET](https://forums.asp.net/).


[![El sitio de las revisiones del libro es ahora accesible para cualquier persona con una conexión a Internet.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Figura 2**: el sitio de las revisiones del libro es ahora accesible para cualquier persona con una conexión a Internet ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Implementar el proyecto de sitio Web de revisión de libro

Al implementar una aplicación ASP.NET que utiliza la compilación automática, como el proyecto de sitio Web BookReviewsWSP, no hay ningún ensamblado compilado en el `Bin` carpeta. Como resultado, los archivos de código fuente de la aplicación web deben implementarse en el entorno de producción. Analicemos este proceso.

Igual que con el proyecto de aplicación Web es conveniente compilar primero la aplicación antes de implementarla. Al compilar un proyecto de sitio Web no crea un ensamblado, busque los errores de tiempo de compilación en la página. Es mejor para buscar ahora estos errores, en lugar de tener un visitante del sitio detectarlos por usted!

Una vez que ha creado correctamente el proyecto, use el cliente de FTP para copiar los archivos siguientes a la carpeta del sitio Web raíz en su proveedor de hospedaje web. Es posible que deba crear la estructura de carpeta correspondiente en el entorno de producción.

> [!NOTE]
> Si ya ha implementado el proyecto pero todavía desea intentar implementar el proyecto BookReviewsWSP de BookReviewsWAP, primero elimine todos los archivos en el servidor web que se han cargado al implementar BookReviewsWAP y, a continuación, implementar los archivos para BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Todo el contenido de la `Styles` carpeta
- Todo el contenido de la `Images` carpeta (y sus subcarpetas, `BookCovers`)
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

Figura 3 muestra FileZilla después de copiar los archivos necesarios. Como puede ver, ASP.NET archivos de código fuente, como `About.aspx.vb`, están presentes en el equipo local (el entorno de desarrollo) y el proveedor de hospedaje web (el entorno de producción) porque los archivos de código deben implementarse al usar automática compilación.


[![Uso de un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hospedaje Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Figura 3**: uso de un cliente FTP para copiar los archivos necesarios desde el escritorio al servidor Web en el proveedor de hospedaje Web ([haga clic aquí para ver imagen en tamaño completo](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


La experiencia del usuario no se ve afectada por el modelo de compilación de la aplicación. Las mismas páginas ASP.NET son accesibles y su apariencia y el mismo comportamiento si se creó el sitio Web mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web.

## <a name="updating-a-web-application-on-production"></a>Actualizar una aplicación Web en producción

Implementación y desarrollo de aplicaciones web no son un proceso único. Por ejemplo, al crear el sitio Web de la reseña de libro compila las diversas páginas y escribió el código que lo acompaña en mi equipo (el entorno de desarrollo). Después de alcanzar un determinado estado estable, implementar mi aplicación para que otros podrían visite el sitio y leer mis opiniones. Pero la implementación no marca el final de mi desarrollo en este sitio. Puedo agregar más reseñas de libros o implementar nuevas características, como permitir que a Mis los visitantes de los libros en pantalla de tasa o dejar sus comentarios. Estas mejoras se pueden desarrollar en el entorno de desarrollo y, cuando haya completado, necesitaría para implementarse. Desarrollo e implementación, por lo tanto, son cíclicos. Desarrollar una aplicación y, a continuación, implementarlo. Aunque el sitio está activo y en producción, se agregan nuevas características y se corrigen los errores con el tiempo, lo que necesita volver a implementar la aplicación. Y así sucesivamente y así sucesivamente.

Como cabría esperar, al volver a implementar una aplicación web, que basta con copiar archivos nuevos y modificados. No es necesario volver a implementar las páginas sin cambios o del lado servidor o cliente admiten archivos (aunque no hay ningún riesgo al hacerlo).

> [!NOTE]
> Una cosa a tener en cuenta cuando se usa la compilación explícita es que, cada vez que agregue una nueva página ASP.NET para el proyecto o realizar cambios relacionados con el código, deberá volver a generar el proyecto, que actualiza el ensamblado en el `Bin` carpeta. Por lo tanto, deberá copiar este ensamblado actualizado en producción al actualizar una aplicación web en producción (junto con el otro contenido nuevo y actualizado).


También comprender todos los cambios en el `Web.config` o los archivos en el `Bin` directory detiene y reinicia el grupo de aplicaciones del sitio Web. Si el estado de sesión se almacena utilizando la `InProc` modo (valor predeterminado), a continuación, los visitantes de su sitio perderá su estado de sesión cada vez que se modifican estos archivos de clave. Para evitar este problema, considere la posibilidad de almacenar la sesión mediante el `StateServer` o `SQLServer` modos. Para obtener más información sobre este tema, consulte [modos de estado de sesión](https://msdn.microsoft.com/library/ms178586.aspx).

Por último, tenga en cuenta que volver a implementar una aplicación puede tardar desde unos segundos hasta varios minutos, según el número y tamaño de los archivos que deben copiarse en el entorno de producción. Durante este tiempo, los usuarios que visitan su sitio pueden experimentar errores o un comportamiento extraño. Puede "desactivar" toda la aplicación mediante la adición de una página denominada `App_Offline.htm` al directorio raíz de la aplicación que se explica a los usuarios que el sitio no está disponible por mantenimiento (o cualquier otra cosa) y se le puede hacer copia de seguridad en breve. Cuando el `App_Offline.htm` archivo está presente, el tiempo de ejecución ASP.NET redirige todas las solicitudes entrantes a esa página.

## <a name="summary"></a>Resumen

Implementar una aplicación web implica copiar los archivos necesarios del entorno de desarrollo al entorno de producción. La forma más común por el que los archivos se transfieren a través de una red es el protocolo de transferencia de archivos (FTP) y la mayoría de los proveedores de host de web admiten el acceso FTP a sus servidores web. En este tutorial, vimos cómo usar a un cliente FTP para implementar los archivos necesarios en el servidor web. Una vez implementado, el sitio Web puede visitar cualquier persona con una conexión a Internet!

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Aplicación\_Offline.htm y estamos trabajando en torno a la característica "Errores de Internet Explorer compatible"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado de sesión](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Anterior](determining-what-files-need-to-be-deployed-vb.md)
> [Siguiente](deploying-your-site-using-visual-studio-vb.md)
