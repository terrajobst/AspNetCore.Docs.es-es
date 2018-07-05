---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Agregar redes sociales para ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: tfitzmac
description: Este capítulo explica cómo integrar su sitio con servicios de redes sociales. En este capítulo, obtendrá información sobre cómo permitir a los usuarios de su sitio Web de marcador o vínculo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4f987c9022056ddfa3cdcac746f688f562d9003e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398406"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Agregar redes sociales a ASP.NET Web Pages (Razor) sitios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo agregar vínculos de redes sociales para Facebook, Twitter, Reddit y Digg a páginas en un sitio Web de ASP.NET Web Pages (Razor) y cómo incluir imágenes Gravatar, tarjetas jugadores de Xbox y fuentes de Twitter.
> 
> Lo que aprenderá:
> 
> - Cómo permitir a los usuarios de su sitio de marcador o vínculo.
> - Cómo agregar una fuente de Twitter.
> - Cómo agregar un Facebook **como** botón a las páginas.
> - Cómo representar imágenes Gravatar.com.
> - Cómo mostrar una tarjeta jugador de Xbox en su sitio.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteca auxiliar de Web ASP.NET (paquete de NuGet)
>   
> 
> En este tutorial también funciona con 3 de ASP.NET Web Pages, excepto las partes que usan la biblioteca auxiliar de Web de ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Vinculación de su sitio Web en sitios de redes sociales

Si personas les gusta algo en su sitio, a menudo desea compartirlo con amigos. Se puede facilitar este proceso al mostrar glifos (iconos) que las personas pueden hacer clic para compartir una página en Digg, Reddit, Facebook, Twitter o sitios similares.

Para mostrar estos glifos, agregue el `LinkSharecode` auxiliar a una página. Las personas que visitan su página de hacer clic en un glifo individual. Si tienen una cuenta con ese sitio de redes sociales, a continuación, puede publicar un vínculo a la página en ese sitio.

![Imagen 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no ha agregado lo - cree una página denominada *ListLinkShare.cshtml* y agregar el siguiente marcado:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    En este ejemplo, cuando el `LinkShare` ejecuciones de aplicación auxiliar, el título de página se pasa como un parámetro, que a su vez pasa el título de página en el sitio de redes sociales. Sin embargo, podría pasar en cualquier cadena que desee. En este ejemplo también especifica qué sitios de redes sociales para incluir en la lista. Puede especificar los sitios de redes sociales que son relevantes para su sitio.
2. Ejecute el *ListLinkShare.cshtml* página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)
3. Haga clic en un glifo para uno de los sitios que se están suscrito. El vínculo le lleva a la página en el sitio de red social seleccionado en el que puede compartir un vínculo. Por ejemplo, si hace clic en el vínculo de Reddit, pasamos a la `submit to reddit` página en el sitio Web de Reddit.

     ![Imagen 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Agregar una Twitter fuente

Para obtener información sobre el uso de una aplicación auxiliar de Twitter que es compatible con la versión actual de la API de Twitter, consulte [aplicación auxiliar de Twitter](../ui-layouts-and-themes/twitter-helper.md). En este ejemplo se muestra cómo escribir su propia aplicación auxiliar de modo que pueda reutilizar fácilmente el código de muchas páginas.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Mostrar un Facebook &quot;como&quot; botón

En algunos casos, la mejor opción es obtener el código directamente desde el proveedor de redes sociales, en lugar de depender de una aplicación auxiliar. Esto es especialmente cierto si el proveedor de red social actualiza sus opciones de forma más rápida de la aplicación auxiliar se actualiza.

Para agregar características de Facebook (por ejemplo, el botón Like) a su sitio, puede recuperar fragmentos de código desde el [developers.facebook.com](https://developers.facebook.com/) sitio. En el sitio de Facebook, usa sus herramientas para generar un fragmento de código que es relevante para el sitio.

El siguiente código resaltado es el código que se recuperó de la herramienta de botón, como en el sitio developers.facebook.com. Debe proporcionar su propio identificador de aplicación.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Representación de una imagen de Gravatar

Un *Gravatar* (un &quot;avatar globalmente reconocido&quot;) es una imagen que se puede usar en varios sitios Web como su avatar &#8212; es decir, una imagen que se representa. Por ejemplo, un Gravatar puede identificar a una persona en un foro, en un comentario en el blog y así sucesivamente. (Puede registrar su propio Gravatar en el sitio Web de Gravatar en [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Si desea mostrar imágenes junto a los nombres de personas o direcciones de correo electrónico en su sitio Web, puede usar la aplicación auxiliar de Gravatar.

En este ejemplo, está usando un Gravatar único que representa a sí mismo. Otra forma de usar un Gravatar es permitir a los usuarios especificar su dirección de Gravatar al registrarse en el sitio. (Puede aprender a permitir a los usuarios registrar en [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) A continuación, siempre que se muestra información de ese usuario, puede agregar el Gravatar a donde se muestra el nombre del usuario.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página web denominada *Gravatar.cshtml*.
3. Agregue el marcado siguiente al archivo: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    El `Gravatar.GetHtml` método muestra la imagen de Gravatar en la página. Para cambiar el tamaño de la imagen, puede incluir un número como segundo parámetro. El tamaño predeterminado es 80. Los números de menos de 80 make la imagen más pequeños. Un número superior a 80 ampliar la imagen.
4. En el `Gravatar.GetHtml` reemplazar métodos, `<Your Gravatar account here>` con la dirección de correo electrónico que usó para la cuenta de Gravatar. (Si no tienes una cuenta de Gravatar, puede utilizar la dirección de correo electrónico de alguien que lo tenga).
5. Ejecute la página en el explorador. La página muestra dos imágenes Gravatar para la dirección de correo electrónico especificada. La segunda imagen es menor que el primero. 

    ![Imagen 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Mostrar una tarjeta jugador de Xbox

Cuando la gente juega Microsoft Xbox en línea, cada usuario tiene un identificador único. Las estadísticas se mantienen para cada jugador en forma de una tarjeta de jugador, que muestra su reputación, la puntuación de jugador y juegos ejecutados recientemente. Si es una jugador de Xbox, puede mostrar la tarjeta de jugador de páginas de su sitio mediante el `GamerCard` auxiliar.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *XboxGamer.cshtml* y agregue el marcado siguiente.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Usa el `GamerCard.GetHtml` propiedad para especificar el alias de la tarjeta de jugador que se mostrará.
3. Ejecute la página en el explorador. La página muestra la tarjeta jugador de Xbox que especificó.

    ![Imagen 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
