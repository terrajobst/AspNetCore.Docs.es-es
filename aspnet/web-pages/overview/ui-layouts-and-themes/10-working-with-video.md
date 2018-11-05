---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Mostrando el vídeo en ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica cómo mostrar el vídeo en un sitio con la página de la sintaxis de Razor de ASP.NET Web Pages.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 8f4b7186ae5c7b7b384ebcb23f7c9ad65caeb0bd
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021045"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Mostrando el vídeo en un sitio Web de ASP.NET Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo usar un Reproductor de vídeo (media) en un sitio Web de ASP.NET Web Pages (Razor) para permitir que los usuarios ver los vídeos que se almacenan en el sitio. ASP.NET Web Pages con sintaxis Razor le permite reproducir Flash (*.swf*), Media Player (*.wmv*) y Silverlight (*.xap*) vídeos.
> 
> Lo que aprenderá:
> 
> - Cómo elegir un Reproductor de vídeo.
> - Cómo agregar vídeo a una página web.
> - Cómo establecer los atributos del Reproductor de vídeo.
> 
> Estos son el código de Razor de ASP.NET las páginas de las características incluidas en el artículo:
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
> Este tutorial también funciona con WebMatrix 3.


## <a name="introduction"></a>Introducción

Es posible que desee mostrar un vídeo en su sitio. Una manera de hacerlo consiste en Vincular a un sitio que ya tiene el vídeo, como YouTube. Si desea insertar un vídeo de estos sitios directamente en sus propias páginas, puede obtener normalmente el código HTML desde el sitio y, a continuación, cópiela en la página. Por ejemplo, en el ejemplo siguiente se muestra cómo insertar un YouTube vídeo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Si desea reproducir un vídeo que se encuentra en su propio sitio Web (no en un sitio público de uso compartido de vídeo), no se puede vincular directamente a ella mediante marcado incrustado similar al siguiente. Sin embargo, puede reproducir vídeos desde su sitio mediante el `Video` aplicación auxiliar, que representa un reproductor multimedia directamente en una página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Elección de un Reproductor de vídeo

Existen muchos formatos de archivos de vídeo, y cada formato normalmente requiere otro reproductor y la otra forma de configurar el Reproductor. En las páginas de Razor de ASP.NET, puede reproducir un vídeo en una página web mediante la `Video` auxiliar. El `Video` auxiliar simplifica el proceso de inserción de vídeos en una página web porque genera automáticamente el `object` y `embed` elementos HTML que se usan normalmente para agregar vídeo a la página.

El `Video` auxiliar admite los siguientes reproductores de medios:

- Adobe Flash
- El Reproductor de Windows Media
- Microsoft Silverlight

### <a name="the-flash-player"></a>El Reproductor de Flash

El `Flash` encargado de la `Video` auxiliares permiten reproducir vídeos Flash (*.swf* archivos) en una página web. Como mínimo, tendrá que proporcionar una ruta de acceso al archivo de vídeo. Si se especifica nada, pero la ruta de acceso, el Reproductor usa los valores predeterminados establecidos por la versión actual de Flash. Configuración predeterminada típica es:

- El vídeo se muestra con su ancho y alto predeterminados y sin un color de fondo.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se repite continuamente hasta que se detiene explícitamente.
- El vídeo se escala para mostrar todo el vídeo, en lugar de recortar el vídeo para ajustarse a un tamaño específico.
- El vídeo se reproduce en una ventana.

### <a name="the-mediaplayer-player"></a>El Reproductor de MediaPlayer

El `MediaPlayer` encargado de la `Video` auxiliar le permite reproducir vídeos de Windows Media (*.wmv* archivos), Windows Media audio (*.wma* archivos) y MP3 (*. mp3* los archivos) en una página web. Debe incluir la ruta de acceso del archivo multimedia para reproducir; todos los demás parámetros son opcionales. Si especifica solo una ruta de acceso, el Reproductor usa la configuración predeterminada establecida por la versión actual de MediaPlayer, tales como:

- El vídeo se muestra con su ancho y alto predeterminados.
- El vídeo se reproduce automáticamente cuando se carga la página.
- El vídeo se reproduce una vez (no se repite).
- El Reproductor muestra el conjunto completo de controles en la interfaz de usuario.
- El vídeo se reproduce en una ventana.

### <a name="the-silverlight-player"></a>El Reproductor de Silverlight

El `Silverlight` encargado de la `Video` auxiliar le permite reproducir el vídeo de Windows Media (*.wmv* archivos), Windows Media Audio (*.wma* archivos) y MP3 (*. mp3* archivos). Debe establecer el parámetro path para que apunte a un paquete de aplicación basada en Silverlight (*.xap* archivo). También debe establecer los parámetros de anchura y altura. Todos los demás parámetros son opcionales. Cuando se usa el Reproductor de Silverlight para vídeo, si se establecen solamente los parámetros necesarios, el Reproductor Silverlight muestra el vídeo sin un color de fondo.

> [!NOTE]
> En caso de que aún no sabe Silverlight: el *.xap* archivo es un archivo comprimido que contiene las instrucciones de diseño en un *.xaml* archivo de código administrado de los ensamblados y recursos opcionales. Puede crear un *.xap* archivo en Visual Studio como un proyecto de aplicación de Silverlight.


El `Silverlight` Reproductor de vídeo que utiliza tanto la configuración proporcionada para el Reproductor y la configuración que se proporciona en el *.xap* archivo.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos de MIME
> 
> Cuando un explorador descarga un archivo, el explorador se asegura de que el tipo de archivo coincide con el tipo MIME que se especifica para el documento que se va a representar. El tipo MIME es el tipo de medios o de tipo de contenido de un archivo. El `Video` auxiliar usa los tipos MIME siguientes:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reproducción de vídeos de Flash (.swf)

Este procedimiento muestra cómo reproducir un vídeo Flash denominado *sample.swf*. El procedimiento se supone que tiene una carpeta denominada *Media* en su sitio y que la *.swf* archivo se encuentra en esa carpeta.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha agregado.
2. En el sitio Web, agregue una página y asígnele el nombre *FlashVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Ejecute la página en un explorador. (Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Se abrirá la página y el vídeo se reproduce automáticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Puede establecer el `quality` parámetro para ver un vídeo Flash a `low`, `autolow`, `autohigh`, `medium`, `high`, y `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Puede cambiar el vídeo Flash para reproducir en un tamaño específico mediante el `scale` parámetro, que puede establecer lo siguiente:

- `showall`. Esto hace que todo el vídeo visible mientras mantiene la relación de aspecto original. Sin embargo, puede terminar con los bordes en cada lado.
- `noorder`. Esto se puede escalar el vídeo mientras se mantiene la relación de aspecto original, pero podría recortarse.
- `exactfit`. Esto hace que todo el vídeo visible sin conservar la relación de aspecto original, pero pueden producirse distorsiones.

Si no especifica un `scale` parámetro, todo el vídeo estará visible y se mantendrá la relación de aspecto original sin cualquier recorte. El ejemplo siguiente muestra cómo usar el `scale` parámetro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

El Reproductor de Flash es compatible con un configuración con nombre de modo de vídeo `windowMode`. Puede establecerlo en `window`, `opaque`, y `transparent`. De forma predeterminada, el `windowMode` está establecido en `window`, que muestra el vídeo en una ventana independiente en la página web. El `opaque` configuración oculta todo detrás del vídeo en la página web. El `transparent` configuración permite que el fondo de la página web se muestra en el vídeo, suponiendo que cualquier parte del vídeo es transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reproducción de MediaPlayer (*.wmv*) vídeos

El siguiente procedimiento muestra cómo reproducir un vídeo de Media de ventana denominado *sample.wmv* que se encuentra en la *Media* carpeta.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *MediaPlayerVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Ejecute la página en un explorador. El vídeo se carga y se reproduce automáticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Puede establecer `playCount` en un entero que indica cuántas veces debe para reproducir el vídeo automáticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

El `uiMode` parámetro le permite especificar qué controles aparecen en la interfaz de usuario. Puede establecer `uiMode` a `invisible`, `none`, `mini`, o `full`. Si no se especifica un `uiMode` parámetro, que será el vídeo se muestra con la ventana de estado, seek barras, controlar los botones y los controles de volumen además de la ventana de vídeo. Estos controles también se mostrará si usa el Reproductor para reproducir un archivo de audio. Este es un ejemplo de cómo usar el `uiMode` parámetro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

De forma predeterminada, audio está activada cuando el vídeo se reproduce. Puede silenciar el audio estableciendo el `mute` parámetro en true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Puede controlar el nivel de audio del vídeo MediaPlayer estableciendo el `volume` parámetro en un valor entre 0 y 100. El valor predeterminado es 50. Por ejemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reproducción de vídeos de Silverlight

Este procedimiento muestra cómo reproducir vídeo contenido en un Silverlight *.xap* página en una carpeta denominada *Media*.

1. Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Crear una nueva página denominada *SilverlightVideo.cshtml*.
3. Agregue el marcado siguiente a la página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Ejecute la página en un explorador. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Información general sobre Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de etiqueta de objeto y la INSERCIÓN de Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Etiqueta PARAM de SDK de Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
