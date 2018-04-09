---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Agregar redes sociales para ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: Este capítulo explica cómo integrar su sitio con los servicios de red sociales. En este capítulo, aprenderá cómo permitir a los usuarios o vínculo de marcador del sitio Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Agregar redes sociales a ASP.NET Web Pages (Razor) sitios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo agregar vínculos de redes sociales de Facebook, Twitter, Reddit y Digg a páginas de un sitio Web de ASP.NET Web Pages (Razor) y cómo incluir fuentes de Twitter, tarjetas de juegos de Xbox y Gravatar imágenes.
> 
> Lo que aprenderá:
> 
> - Cómo permitir a los usuarios o vínculo de marcador su sitio.
> - Cómo agregar una fuente de Twitter.
> - Cómo agregar un Facebook **como** botón a las páginas.
> - Cómo representar imágenes Gravatar.com.
> - Cómo mostrar una tarjeta de juegos de Xbox en su sitio.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteca auxiliar de Web ASP.NET (paquete de NuGet)
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 3, salvo los elementos que utilizan la biblioteca de aplicación auxiliar de Web de ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Vinculación de su sitio Web en los sitios de redes sociales

Si las personas les gusta algo en su sitio, a menudo desea compartirlo con amigos. Puede facilitar este proceso al mostrar glifos (iconos) que se hace clic para compartir una página en Digg, Reddit, Facebook, Twitter o sitios similares.

Para mostrar estos glifos, agregue el `LinkSharecode` auxiliar a una página. Personas que visitan la página hacer clic en un glifo individual. Si tienen una cuenta con ese sitio de red social, a continuación, puede publicar un vínculo a la página en ese sitio.

![Imagen 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no ha agregado lo - crear una página denominada *ListLinkShare.cshtml* y agregar el siguiente marcado:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    En este ejemplo, cuando el `LinkShare` auxiliar se ejecuta, el título de página se pasa como un parámetro, que a su vez pasa el título de página en el sitio de red social. Sin embargo, podría pasar en cualquier cadena que se desee. En este ejemplo también especifica qué sitios de redes sociales para incluir en la lista. Puede especificar los sitios de redes sociales que son relevantes para su sitio.
2. Ejecute el *ListLinkShare.cshtml* página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
3. Haga clic en un glifo para uno de los sitios que ha iniciado sesión en. El vínculo le lleva a la página en el sitio de red social seleccionado donde se pueden compartir un vínculo. Por ejemplo, si hace clic en el vínculo de Reddit, llega a la `submit to reddit` página en el sitio Web Reddit.

     ![Imagen 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Agregar un Twitter fuente

Para obtener información sobre el uso de una aplicación auxiliar de Twitter que sea compatible con la versión actual de la API de Twitter, vea [aplicación auxiliar de Twitter](../ui-layouts-and-themes/twitter-helper.md). En este ejemplo se muestra cómo escribir su propia aplicación auxiliar de modo que pueda reutilizar fácilmente el código de muchas páginas.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Mostrar un Facebook &quot;como&quot; botón

En algunos casos, la mejor opción es obtener el código directamente desde el proveedor de redes sociales, en lugar de confiar en una aplicación auxiliar. Esto es especialmente cierto si el proveedor de red social actualiza sus opciones más rápidamente de lo que se actualiza la aplicación auxiliar.

Para agregar características de Facebook (por ejemplo, el botón Like) a su sitio, puede recuperar los fragmentos de código de la [developers.facebook.com](https://developers.facebook.com/) sitio. En el sitio de Facebook, usando sus herramientas para generar un fragmento de código que es relevante para el sitio.

El código resaltado siguiente es el código que se recuperó de la herramienta como botón en el sitio developers.facebook.com. Debe proporcionar su propio identificador de aplicación.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Representación de una imagen Gravatar

A *Gravatar* (un &quot;avatar globalmente reconocido&quot;) es una imagen que se puede usar en varios sitios Web como su avatar &#8212; es decir, una imagen que le represente. Por ejemplo, un Gravatar puede identificar a una persona con una entrada de foro, un comentario de blog y así sucesivamente. (Puede registrar su propio Gravatar en el sitio Web de Gravatar en [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Si desea mostrar imágenes junto a los nombres de personas o direcciones de correo electrónico en el sitio Web, puede usar la aplicación auxiliar Gravatar.

En este ejemplo, está usando un Gravatar único que representa a sí mismo. Otra manera de usar un Gravatar es permitir a los usuarios especificar su dirección Gravatar cuando registran en su sitio. (Puede obtener información sobre cómo permitir a los usuarios registrar en [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) A continuación, cada vez que se muestra información para ese usuario, puede agregar simplemente la Gravatar a donde se muestra el nombre del usuario.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página web denominada *Gravatar.cshtml*.
3. Agregue el siguiente marcado al archivo: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    El `Gravatar.GetHtml` método muestra la imagen de Gravatar en la página. Para cambiar el tamaño de la imagen, puede incluir un número como un segundo parámetro. El tamaño predeterminado es 80. Los números de inferior a 80 hacer la imagen más pequeños. Un número superior a 80 ampliar la imagen.
4. En el `Gravatar.GetHtml` reemplazar métodos, `<Your Gravatar account here>` con la dirección de correo electrónico que usas para su cuenta de Gravatar. (Si no tiene una cuenta de Gravatar, puede utilizar la dirección de correo electrónico de alguien que lo tiene).
5. Ejecute la página en el explorador. La página muestra dos imágenes de Gravatar para la dirección de correo electrónico que especificó. La segunda imagen es menor que el primero. 

    ![Imagen 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Mostrar una tarjeta de juegos de Xbox

Cuando las personas jugar Microsoft Xbox en línea, cada usuario tiene un identificador único. Las estadísticas se mantienen para cada jugador en forma de una tarjeta jugador, que muestra su reputación, puntuación jugador y juegos ejecutados recientemente. Si eres una jugador Xbox, puede mostrar la tarjeta jugador en páginas en el sitio mediante el `GamerCard` auxiliar.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *XboxGamer.cshtml* y agregue el siguiente marcado.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Usa el `GamerCard.GetHtml` propiedad para especificar el alias de la tarjeta jugador para mostrarse.
3. Ejecute la página en el explorador. La página muestra la tarjeta de juegos de Xbox que especificó.

    ![Imagen 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
