---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[¿Cómo I:]  Caché de una página ASP.NET basada en la información en el encabezado HTTP | Documentos de Microsoft"
author: rick-anderson
description: "En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET basada en información de encabezado HTTP de la página. En primer lugar, el potencial i HTTP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: ce5ea10396d0fe31d72425e2431102a0cb0c3bd0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="ed68c-104">[¿Cómo I:]  Caché de una página ASP.NET basada en la información en el encabezado HTTP</span><span class="sxs-lookup"><span data-stu-id="ed68c-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="ed68c-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ed68c-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ed68c-106">En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET basada en información de encabezado HTTP de la página.</span><span class="sxs-lookup"><span data-stu-id="ed68c-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="ed68c-107">En primer lugar, se revisan los posibles valores del encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed68c-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="ed68c-108">A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "accept-language", un encabezado HTTP, para controlar el almacenamiento en caché según el idioma del explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="ed68c-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="ed68c-109">La página de ejemplos se ve en Internet Explorer que está establecida en inglés y, a continuación, en FireFox que está configurado para usar el francés.</span><span class="sxs-lookup"><span data-stu-id="ed68c-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="ed68c-110">Por último, se explica la opción para mover la definición de la memoria caché para un CacheProfile en el archivo web.config.</span><span class="sxs-lookup"><span data-stu-id="ed68c-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="ed68c-111">&#9654; Vea el vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="ed68c-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
