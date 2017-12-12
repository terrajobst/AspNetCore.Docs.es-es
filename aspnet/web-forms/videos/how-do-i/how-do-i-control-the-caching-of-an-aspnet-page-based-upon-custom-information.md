---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: "[¿Cómo I:] El almacenamiento en caché de una página ASP.NET en función de información personalizada de control | Documentos de Microsoft"
author: rick-anderson
description: "En este vídeo Chris Pels muestra cómo controlar los criterios para almacenar en caché una página ASP.NET en función de la información personalizada. Se crea una página de ejemplo y, a continuación, el o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="a150f-104">[¿Cómo I:] El almacenamiento en caché de una página ASP.NET en función de información personalizada de control</span><span class="sxs-lookup"><span data-stu-id="a150f-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="a150f-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a150f-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a150f-106">En este vídeo Chris Pels muestra cómo controlar los criterios para almacenar en caché una página ASP.NET en función de la información personalizada.</span><span class="sxs-lookup"><span data-stu-id="a150f-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="a150f-107">Se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByCustom que contiene un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="a150f-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="a150f-108">A continuación, se reemplaza el método GetVaryCustomByString() en el módulo de global.asax que proporciona el control del atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="a150f-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="a150f-109">En ese método se devuelve una cadena que identifica de forma única la versión en caché de la página.</span><span class="sxs-lookup"><span data-stu-id="a150f-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="a150f-110">Por último, hay una explicación sobre cómo el almacenamiento en caché con un valor personalizado se puede usar de varias maneras para un sitio web.</span><span class="sxs-lookup"><span data-stu-id="a150f-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="a150f-111">&#9654; Vea el vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="a150f-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
