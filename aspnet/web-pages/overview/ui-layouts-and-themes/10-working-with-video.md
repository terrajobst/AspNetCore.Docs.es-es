---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "Mostrando el vídeo en ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft"
author: tfitzmac
description: "En este capítulo se explica cómo mostrar el vídeo en un sitio con la página de la sintaxis de Razor de ASP.NET Web Pages."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: a14659997d86d1b5cf5381e21e997c1a03a3f57c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Mostrando el vídeo en un sitio de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo explica cómo utilizar un Reproductor de vídeo (media) en un sitio Web de ASP.NET Web Pages (Razor) para permitir a los usuarios ver vídeos que se almacenan en el sitio. ASP.NET Web Pages con sintaxis Razor le permite reproducir Flash (*.swf*), el Reproductor de Media (*.wmv*) y Silverlight (*.xap*) vídeos.
> 
> Lo que aprenderá:
> 
> - Cómo elegir un Reproductor de vídeo.
> - Cómo agregar vídeo a una página web.
> - Cómo establecer atributos de Reproductor de vídeo.
> 
> Se trata de ASP.NET Razor páginas características introducidas en el artículo:
> 
> - El `Video` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con 3 de WebMatrix.


## <a name="introduction"></a>Introducción

Puede visualizar un vídeo en el sitio. Una manera de hacerlo consiste en Vincular a un sitio que ya tiene el vídeo, como YouTube. Si desea incrustar un vídeo de estos sitios directamente en sus propias páginas, puede obtener normalmente marcado HTML desde el sitio y, a continuación, copiarlo en la página. Por ejemplo, en el ejemplo siguiente se muestra cómo incrustar un YouTube vídeo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Si desea reproducir un vídeo que se encuentra en su propio sitio Web (no en un sitio de compartir vídeos público), no puede vincular directamente a él utilizando incrustado marcado similar al siguiente. Sin embargo, puede reproducir vídeos desde su sitio mediante el `Video` auxiliar, que presenta un reproductor multimedia directamente en una página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Elegir un Reproductor de vídeo

Hay una gran cantidad de formatos de archivos de vídeo, y cada formato normalmente requiere otro reproductor y otra forma de configurar el Reproductor. En las páginas de ASP.NET Razor, puede reproducir un vídeo en una página web mediante la `Video` auxiliar. El `Video` auxiliar simplifica el proceso de incrustación vídeos en una página web, porque lo genera automáticamente el `object` y `embed` elementos HTML que se usan normalmente para agregar vídeo a la página.

El `Video` auxiliar es compatible con los siguientes reproductores de medios:

- Adobe Flash
- Reproductor de Windows Media
- Microsoft Silverlight

### <a name="the-flash-player"></a>El Reproductor Flash

El `Flash` Reproductor de la `Video` auxiliar le permite reproducir vídeos Flash (*.swf* archivos) en una página web. Como mínimo, tendrá que proporcionar una ruta de acceso para el archivo de vídeo. Si especifica únicamente la ruta de acceso, el Reproductor usa los valores predeterminados establecidos por la versión actual de Flash. Valores predeterminados típicos son:

- El vídeo se muestra con su ancho y alto predeterminados y sin un color de fondo.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se repite continuamente hasta que se detiene explícitamente.
- El vídeo se escala para mostrar todo el vídeo, en lugar de recortar el vídeo para ajustarse a un tamaño específico.
- El vídeo se reproduce en una ventana.

### <a name="the-mediaplayer-player"></a>El Reproductor del Reproductor de Media

El `MediaPlayer` Reproductor de la `Video` auxiliar permite reproducir vídeos de Windows Media (*.wmv* archivos), Windows Media audio (*.wma* archivos) y MP3 (*. mp3* archivos) en una página web. Debe incluir la ruta de acceso del archivo multimedia para reproducir; todos los demás parámetros son opcionales. Si especifica solo una ruta de acceso, el Reproductor usa la configuración predeterminada establecida por la versión actual del Reproductor de Media, como:

- El vídeo se muestra con su ancho y alto predeterminados.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se reproduce una vez (no se repite).
- El Reproductor muestra el conjunto completo de controles en la interfaz de usuario.
- El vídeo se reproduce en una ventana.

### <a name="the-silverlight-player"></a>El Reproductor de Silverlight

El `Silverlight` Reproductor de la `Video` auxiliar permite reproducir vídeo de Windows Media (*.wmv* archivos), Windows Media Audio (*.wma* archivos) y MP3 (*. mp3* archivos). Debe establecer el parámetro de ruta de acceso para que apunte a un paquete de aplicación basada en Silverlight (*.xap* archivo). También debe establecer los parámetros de ancho y alto. Todos los demás parámetros son opcionales. Cuando se usa el Reproductor de Silverlight para vídeo, si establece solamente los parámetros necesarios, el Reproductor de Silverlight muestra el vídeo sin un color de fondo.

> [!NOTE]
> En caso de que aún no sabe Silverlight: el *.xap* archivo es un archivo comprimido que contiene instrucciones de diseño en un *.xaml* de archivos, código administrado de los ensamblados y recursos opcionales. Puede crear un *.xap* archivo en Visual Studio como un proyecto de aplicación de Silverlight.


El `Silverlight` Reproductor de vídeo que utiliza tanto la configuración que proporciona para que el Reproductor y la configuración que se proporciona en el *.xap* archivo.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos de MIME
> 
> Cuando un explorador descarga un archivo, el explorador se asegura de que el tipo de archivo coincide con el tipo MIME especificado para el documento que se va a representar. El tipo MIME es el tipo de medios o de tipo de contenido de un archivo. El `Video` auxiliar utiliza los siguientes tipos MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reproducción de vídeos de Flash (.swf)

Este procedimiento muestra cómo reproducir un vídeo Flash denominado *sample.swf*. El procedimiento se da por supuesto que tiene una carpeta denominada *Media* en su sitio y que la *.swf* archivo se encuentra en esa carpeta.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no se ha agregado.
2. En el sitio Web, agregue una página y asígnele el nombre *FlashVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Ejecute la página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Se abrirá la página y el vídeo se reproduce automáticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Puede establecer la `quality` parámetro para ver un vídeo Flash para `low`, `autolow`, `autohigh`, `medium`, `high`, y `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Puede cambiar el Flash vídeo se reproduce en un tamaño específico utilizando la `scale` parámetro, que puede establecer para lo siguiente:

- `showall`. Esto hace que todo el vídeo visible mientras se mantiene la relación de aspecto original. Sin embargo, podría acabar con bordes en cada lado.
- `noorder`. Esto ajusta el vídeo mientras se mantiene la relación de aspecto original, pero podría recortarse.
- `exactfit`. Esto hace que todo el vídeo visible sin preservar la relación de aspecto original, pero podría producir la distorsión.

Si no se especifica un `scale` parámetro, todo el vídeo estarán visible y se mantendrá la relación de aspecto original sin cualquier recorte. En el ejemplo siguiente se muestra cómo utilizar el `scale` parámetro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

El Reproductor Flash es compatible con un configuración con nombre de modo de vídeo `windowMode`. También puede establecerlo en `window`, `opaque`, y `transparent`. De forma predeterminada, el `windowMode` se establece en `window`, que muestra el vídeo en una ventana independiente en la página web. El `opaque` configuración oculta todo el contenido tras el vídeo en la página web. El `transparent` configuración permite que el fondo de la página web se ven a través de vídeo, suponiendo que cualquier parte del vídeo es transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reproducir el Reproductor de Media (*.wmv*) vídeos

El siguiente procedimiento muestra cómo reproducir un vídeo de medios de ventana denominado *sample.wmv* que se encuentra en la *Media* carpeta.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *MediaPlayerVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Ejecute la página en un explorador. El vídeo se carga y se reproduce automáticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Puede establecer `playCount` en un entero que indica el número de veces que se reproduzca el vídeo automáticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

El `uiMode` parámetro le permite especificar qué controles aparecen en la interfaz de usuario. Puede establecer `uiMode` a `invisible`, `none`, `mini`, o `full`. Si no se especifica un `uiMode` parámetro, será el vídeo se muestra con la ventana de estado, buscar barras, controlar los botones y controles de volumen además de la ventana de vídeo. Estos controles se mostrará también si usa el Reproductor para reproducir un archivo de audio. Este es un ejemplo de cómo usar el `uiMode` parámetro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

De forma predeterminada, el audio está activada cuando el vídeo se reproduce. Puede silenciar el audio estableciendo la `mute` parámetro en true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Puede controlar el nivel de audio del vídeo MediaPlayer estableciendo la `volume` parámetro en un valor entre 0 y 100. El valor predeterminado es 50. Por ejemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reproducción de vídeos de Silverlight

Este procedimiento muestra cómo reproducir vídeo contenido en un Silverlight *.xap* página en una carpeta denominada *Media*.

1. Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *SilverlightVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Ejecute la página en un explorador. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Información general de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de etiqueta de objeto y EMBED de Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Etiquetas PARAM de SDK de Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
