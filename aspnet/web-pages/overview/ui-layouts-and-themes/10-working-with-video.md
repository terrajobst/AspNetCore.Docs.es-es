---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Mostrando el vídeo en ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: En este capítulo se explica cómo mostrar el vídeo en un sitio con la página de la sintaxis de Razor de ASP.NET Web Pages.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153685"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="8ade7-103">Mostrando el vídeo en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="8ade7-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="8ade7-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8ade7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8ade7-105">Este artículo explica cómo utilizar un Reproductor de vídeo (media) en un sitio Web de ASP.NET Web Pages (Razor) para permitir a los usuarios ver vídeos que se almacenan en el sitio.</span><span class="sxs-lookup"><span data-stu-id="8ade7-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="8ade7-106">ASP.NET Web Pages con sintaxis Razor le permite reproducir Flash (*.swf*), el Reproductor de Media (*.wmv*) y Silverlight (*.xap*) vídeos.</span><span class="sxs-lookup"><span data-stu-id="8ade7-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="8ade7-107">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="8ade7-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8ade7-108">Cómo elegir un Reproductor de vídeo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-108">How to choose a video player.</span></span>
> - <span data-ttu-id="8ade7-109">Cómo agregar vídeo a una página web.</span><span class="sxs-lookup"><span data-stu-id="8ade7-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="8ade7-110">Cómo establecer atributos de Reproductor de vídeo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="8ade7-111">Se trata de ASP.NET Razor páginas características introducidas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="8ade7-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="8ade7-112">El `Video` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="8ade7-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8ade7-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="8ade7-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8ade7-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="8ade7-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="8ade7-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="8ade7-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="8ade7-116">Este tutorial también funciona con 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8ade7-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="8ade7-117">Introducción</span><span class="sxs-lookup"><span data-stu-id="8ade7-117">Introduction</span></span>

<span data-ttu-id="8ade7-118">Puede visualizar un vídeo en el sitio.</span><span class="sxs-lookup"><span data-stu-id="8ade7-118">You might want to display a video on your site.</span></span> <span data-ttu-id="8ade7-119">Una manera de hacerlo consiste en Vincular a un sitio que ya tiene el vídeo, como YouTube.</span><span class="sxs-lookup"><span data-stu-id="8ade7-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="8ade7-120">Si desea incrustar un vídeo de estos sitios directamente en sus propias páginas, puede obtener normalmente marcado HTML desde el sitio y, a continuación, copiarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="8ade7-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="8ade7-121">Por ejemplo, en el ejemplo siguiente se muestra cómo incrustar un YouTube vídeo:</span><span class="sxs-lookup"><span data-stu-id="8ade7-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="8ade7-122">Si desea reproducir un vídeo que se encuentra en su propio sitio Web (no en un sitio de compartir vídeos público), no puede vincular directamente a él utilizando incrustado marcado similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="8ade7-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="8ade7-123">Sin embargo, puede reproducir vídeos desde su sitio mediante el `Video` auxiliar, que presenta un reproductor multimedia directamente en una página.</span><span class="sxs-lookup"><span data-stu-id="8ade7-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="8ade7-124">Elegir un Reproductor de vídeo</span><span class="sxs-lookup"><span data-stu-id="8ade7-124">Choosing a Video Player</span></span>

<span data-ttu-id="8ade7-125">Hay una gran cantidad de formatos de archivos de vídeo, y cada formato normalmente requiere otro reproductor y otra forma de configurar el Reproductor.</span><span class="sxs-lookup"><span data-stu-id="8ade7-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="8ade7-126">En las páginas de ASP.NET Razor, puede reproducir un vídeo en una página web mediante la `Video` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="8ade7-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="8ade7-127">El `Video` auxiliar simplifica el proceso de incrustación vídeos en una página web, porque lo genera automáticamente el `object` y `embed` elementos HTML que se usan normalmente para agregar vídeo a la página.</span><span class="sxs-lookup"><span data-stu-id="8ade7-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="8ade7-128">El `Video` auxiliar es compatible con los siguientes reproductores de medios:</span><span class="sxs-lookup"><span data-stu-id="8ade7-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="8ade7-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="8ade7-129">Adobe Flash</span></span>
- <span data-ttu-id="8ade7-130">Reproductor de Windows Media</span><span class="sxs-lookup"><span data-stu-id="8ade7-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="8ade7-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="8ade7-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="8ade7-132">El Reproductor Flash</span><span class="sxs-lookup"><span data-stu-id="8ade7-132">The Flash Player</span></span>

<span data-ttu-id="8ade7-133">El `Flash` Reproductor de la `Video` auxiliar le permite reproducir vídeos Flash (*.swf* archivos) en una página web.</span><span class="sxs-lookup"><span data-stu-id="8ade7-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="8ade7-134">Como mínimo, tendrá que proporcionar una ruta de acceso para el archivo de vídeo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="8ade7-135">Si especifica únicamente la ruta de acceso, el Reproductor usa los valores predeterminados establecidos por la versión actual de Flash.</span><span class="sxs-lookup"><span data-stu-id="8ade7-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="8ade7-136">Valores predeterminados típicos son:</span><span class="sxs-lookup"><span data-stu-id="8ade7-136">Typical default settings are:</span></span>

- <span data-ttu-id="8ade7-137">El vídeo se muestra con su ancho y alto predeterminados y sin un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="8ade7-138">El vídeo se reproduce automáticamente cuando se carga la página.</span><span class="sxs-lookup"><span data-stu-id="8ade7-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="8ade7-139">El vídeo se repite continuamente hasta que se detiene explícitamente.</span><span class="sxs-lookup"><span data-stu-id="8ade7-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="8ade7-140">El vídeo se escala para mostrar todo el vídeo, en lugar de recortar el vídeo para ajustarse a un tamaño específico.</span><span class="sxs-lookup"><span data-stu-id="8ade7-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="8ade7-141">El vídeo se reproduce en una ventana.</span><span class="sxs-lookup"><span data-stu-id="8ade7-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="8ade7-142">El Reproductor del Reproductor de Media</span><span class="sxs-lookup"><span data-stu-id="8ade7-142">The MediaPlayer Player</span></span>

<span data-ttu-id="8ade7-143">El `MediaPlayer` Reproductor de la `Video` auxiliar permite reproducir vídeos de Windows Media (*.wmv* archivos), Windows Media audio (*.wma* archivos) y MP3 (*. mp3* archivos) en una página web.</span><span class="sxs-lookup"><span data-stu-id="8ade7-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="8ade7-144">Debe incluir la ruta de acceso del archivo multimedia para reproducir; todos los demás parámetros son opcionales.</span><span class="sxs-lookup"><span data-stu-id="8ade7-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="8ade7-145">Si especifica solo una ruta de acceso, el Reproductor usa la configuración predeterminada establecida por la versión actual del Reproductor de Media, como:</span><span class="sxs-lookup"><span data-stu-id="8ade7-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="8ade7-146">El vídeo se muestra con su ancho y alto predeterminados.</span><span class="sxs-lookup"><span data-stu-id="8ade7-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="8ade7-147">El vídeo se reproduce automáticamente cuando se carga la página.</span><span class="sxs-lookup"><span data-stu-id="8ade7-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="8ade7-148">El vídeo se reproduce una vez (no se repite).</span><span class="sxs-lookup"><span data-stu-id="8ade7-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="8ade7-149">El Reproductor muestra el conjunto completo de controles en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="8ade7-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="8ade7-150">El vídeo se reproduce en una ventana.</span><span class="sxs-lookup"><span data-stu-id="8ade7-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="8ade7-151">El Reproductor de Silverlight</span><span class="sxs-lookup"><span data-stu-id="8ade7-151">The Silverlight Player</span></span>

<span data-ttu-id="8ade7-152">El `Silverlight` Reproductor de la `Video` auxiliar permite reproducir vídeo de Windows Media (*.wmv* archivos), Windows Media Audio (*.wma* archivos) y MP3 (*. mp3* archivos).</span><span class="sxs-lookup"><span data-stu-id="8ade7-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="8ade7-153">Debe establecer el parámetro de ruta de acceso para que apunte a un paquete de aplicación basada en Silverlight (*.xap* archivo).</span><span class="sxs-lookup"><span data-stu-id="8ade7-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="8ade7-154">También debe establecer los parámetros de ancho y alto.</span><span class="sxs-lookup"><span data-stu-id="8ade7-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="8ade7-155">Todos los demás parámetros son opcionales.</span><span class="sxs-lookup"><span data-stu-id="8ade7-155">All other parameters are optional.</span></span> <span data-ttu-id="8ade7-156">Cuando se usa el Reproductor de Silverlight para vídeo, si establece solamente los parámetros necesarios, el Reproductor de Silverlight muestra el vídeo sin un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="8ade7-157">En caso de que aún no sabe Silverlight: el *.xap* archivo es un archivo comprimido que contiene instrucciones de diseño en un *.xaml* de archivos, código administrado de los ensamblados y recursos opcionales.</span><span class="sxs-lookup"><span data-stu-id="8ade7-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="8ade7-158">Puede crear un *.xap* archivo en Visual Studio como un proyecto de aplicación de Silverlight.</span><span class="sxs-lookup"><span data-stu-id="8ade7-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="8ade7-159">El `Silverlight` Reproductor de vídeo que utiliza tanto la configuración que proporciona para que el Reproductor y la configuración que se proporciona en el *.xap* archivo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="8ade7-160">Tipos de MIME</span><span class="sxs-lookup"><span data-stu-id="8ade7-160">MIME Types</span></span>
> 
> <span data-ttu-id="8ade7-161">Cuando un explorador descarga un archivo, el explorador se asegura de que el tipo de archivo coincide con el tipo MIME especificado para el documento que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="8ade7-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="8ade7-162">El tipo MIME es el tipo de medios o de tipo de contenido de un archivo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="8ade7-163">El `Video` auxiliar utiliza los siguientes tipos MIME:</span><span class="sxs-lookup"><span data-stu-id="8ade7-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="8ade7-164">Reproducción de vídeos de Flash (.swf)</span><span class="sxs-lookup"><span data-stu-id="8ade7-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="8ade7-165">Este procedimiento muestra cómo reproducir un vídeo Flash denominado *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="8ade7-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="8ade7-166">El procedimiento se da por supuesto que tiene una carpeta denominada *Media* en su sitio y que la *.swf* archivo se encuentra en esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="8ade7-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="8ade7-167">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="8ade7-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="8ade7-168">En el sitio Web, agregue una página y asígnele el nombre *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ade7-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="8ade7-169">Agregue el marcado siguiente a la página:</span><span class="sxs-lookup"><span data-stu-id="8ade7-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="8ade7-170">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="8ade7-170">Run the page in a browser.</span></span> <span data-ttu-id="8ade7-171">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Se abrirá la página y el vídeo se reproduce automáticamente.</span><span class="sxs-lookup"><span data-stu-id="8ade7-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="8ade7-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="8ade7-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="8ade7-173">Puede establecer la `quality` parámetro para ver un vídeo Flash para `low`, `autolow`, `autohigh`, `medium`, `high`, y `best`:</span><span class="sxs-lookup"><span data-stu-id="8ade7-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="8ade7-174">Puede cambiar el Flash vídeo se reproduce en un tamaño específico utilizando la `scale` parámetro, que puede establecer para lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8ade7-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="8ade7-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-175">`showall`.</span></span> <span data-ttu-id="8ade7-176">Esto hace que todo el vídeo visible mientras se mantiene la relación de aspecto original.</span><span class="sxs-lookup"><span data-stu-id="8ade7-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="8ade7-177">Sin embargo, podría acabar con bordes en cada lado.</span><span class="sxs-lookup"><span data-stu-id="8ade7-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="8ade7-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-178">`noorder`.</span></span> <span data-ttu-id="8ade7-179">Esto ajusta el vídeo mientras se mantiene la relación de aspecto original, pero podría recortarse.</span><span class="sxs-lookup"><span data-stu-id="8ade7-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="8ade7-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-180">`exactfit`.</span></span> <span data-ttu-id="8ade7-181">Esto hace que todo el vídeo visible sin preservar la relación de aspecto original, pero podría producir la distorsión.</span><span class="sxs-lookup"><span data-stu-id="8ade7-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="8ade7-182">Si no se especifica un `scale` parámetro, todo el vídeo estarán visible y se mantendrá la relación de aspecto original sin cualquier recorte.</span><span class="sxs-lookup"><span data-stu-id="8ade7-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="8ade7-183">En el ejemplo siguiente se muestra cómo utilizar el `scale` parámetro:</span><span class="sxs-lookup"><span data-stu-id="8ade7-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="8ade7-184">El Reproductor Flash es compatible con un configuración con nombre de modo de vídeo `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="8ade7-185">También puede establecerlo en `window`, `opaque`, y `transparent`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="8ade7-186">De forma predeterminada, el `windowMode` se establece en `window`, que muestra el vídeo en una ventana independiente en la página web.</span><span class="sxs-lookup"><span data-stu-id="8ade7-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="8ade7-187">El `opaque` configuración oculta todo el contenido tras el vídeo en la página web.</span><span class="sxs-lookup"><span data-stu-id="8ade7-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="8ade7-188">El `transparent` configuración permite que el fondo de la página web se ven a través de vídeo, suponiendo que cualquier parte del vídeo es transparente.</span><span class="sxs-lookup"><span data-stu-id="8ade7-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="8ade7-189">Reproducir el Reproductor de Media (*.wmv*) vídeos</span><span class="sxs-lookup"><span data-stu-id="8ade7-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="8ade7-190">El siguiente procedimiento muestra cómo reproducir un vídeo de medios de ventana denominado *sample.wmv* que se encuentra en la *Media* carpeta.</span><span class="sxs-lookup"><span data-stu-id="8ade7-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="8ade7-191">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="8ade7-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="8ade7-192">Crear una nueva página denominada *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ade7-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="8ade7-193">Agregue el marcado siguiente a la página:</span><span class="sxs-lookup"><span data-stu-id="8ade7-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="8ade7-194">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="8ade7-194">Run the page in a browser.</span></span> <span data-ttu-id="8ade7-195">El vídeo se carga y se reproduce automáticamente.</span><span class="sxs-lookup"><span data-stu-id="8ade7-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="8ade7-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="8ade7-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="8ade7-197">Puede establecer `playCount` en un entero que indica el número de veces que se reproduzca el vídeo automáticamente:</span><span class="sxs-lookup"><span data-stu-id="8ade7-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="8ade7-198">El `uiMode` parámetro le permite especificar qué controles aparecen en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="8ade7-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="8ade7-199">Puede establecer `uiMode` a `invisible`, `none`, `mini`, o `full`.</span><span class="sxs-lookup"><span data-stu-id="8ade7-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="8ade7-200">Si no se especifica un `uiMode` parámetro, será el vídeo se muestra con la ventana de estado, buscar barras, controlar los botones y controles de volumen además de la ventana de vídeo.</span><span class="sxs-lookup"><span data-stu-id="8ade7-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="8ade7-201">Estos controles se mostrará también si usa el Reproductor para reproducir un archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="8ade7-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="8ade7-202">Este es un ejemplo de cómo usar el `uiMode` parámetro:</span><span class="sxs-lookup"><span data-stu-id="8ade7-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="8ade7-203">De forma predeterminada, el audio está activada cuando el vídeo se reproduce.</span><span class="sxs-lookup"><span data-stu-id="8ade7-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="8ade7-204">Puede silenciar el audio estableciendo la `mute` parámetro en true:</span><span class="sxs-lookup"><span data-stu-id="8ade7-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="8ade7-205">Puede controlar el nivel de audio del vídeo MediaPlayer estableciendo la `volume` parámetro en un valor entre 0 y 100.</span><span class="sxs-lookup"><span data-stu-id="8ade7-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="8ade7-206">El valor predeterminado es 50.</span><span class="sxs-lookup"><span data-stu-id="8ade7-206">The default value is 50.</span></span> <span data-ttu-id="8ade7-207">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8ade7-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="8ade7-208">Reproducción de vídeos de Silverlight</span><span class="sxs-lookup"><span data-stu-id="8ade7-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="8ade7-209">Este procedimiento muestra cómo reproducir vídeo contenido en un Silverlight *.xap* página en una carpeta denominada *Media*.</span><span class="sxs-lookup"><span data-stu-id="8ade7-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="8ade7-210">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="8ade7-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="8ade7-211">Crear una nueva página denominada *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8ade7-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="8ade7-212">Agregue el marcado siguiente a la página:</span><span class="sxs-lookup"><span data-stu-id="8ade7-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="8ade7-213">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="8ade7-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="8ade7-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="8ade7-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="8ade7-215">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8ade7-215">Additional Resources</span></span>


<span data-ttu-id="8ade7-216">[Información general de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="8ade7-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="8ade7-217">Atributos de etiqueta de objeto y EMBED de Flash</span><span class="sxs-lookup"><span data-stu-id="8ade7-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="8ade7-218">[Etiquetas PARAM de SDK de Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="8ade7-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
