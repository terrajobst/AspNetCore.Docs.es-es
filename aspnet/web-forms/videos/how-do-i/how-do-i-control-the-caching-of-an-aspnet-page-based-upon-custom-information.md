---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[¿Cómo lo hago?:] El almacenamiento en caché de una página ASP.NET en función de información personalizada de control | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo controlar los criterios para almacenar en caché una página ASP.NET en función de información personalizada. Se crea una página de ejemplo y, a continuación, el deseo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376025"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="85834-104">[¿Cómo lo hago?:] El almacenamiento en caché de una página ASP.NET en función de información personalizada de control</span><span class="sxs-lookup"><span data-stu-id="85834-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="85834-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="85834-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="85834-106">En este vídeo Chris Pels muestra cómo controlar los criterios para almacenar en caché una página ASP.NET en función de información personalizada.</span><span class="sxs-lookup"><span data-stu-id="85834-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="85834-107">Se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByCustom que contiene un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="85834-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="85834-108">A continuación, se invalida el método GetVaryCustomByString() en el módulo de global.asax que proporciona el control del atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="85834-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="85834-109">En ese método se devuelve una cadena que identifica la versión en caché de la página.</span><span class="sxs-lookup"><span data-stu-id="85834-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="85834-110">Por último, hay una explicación sobre cómo almacenar en caché con un valor personalizado se puede usar de varias maneras para un sitio web.</span><span class="sxs-lookup"><span data-stu-id="85834-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="85834-111">&#9654;Vea el vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="85834-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
