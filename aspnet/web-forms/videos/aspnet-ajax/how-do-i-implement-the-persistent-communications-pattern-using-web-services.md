---
uid: web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
title: '[¿Cómo lo hago?:] ¿Implementar el patrón de comunicaciones persistente mediante servicios Web? | Microsoft Docs'
author: JoeStagner
description: En un sitio Web tradicional del explorador y el servidor no mantienen una comunicación continua, pero comunican solo como respuesta al usuario realizar un acto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/22/2007
ms.topic: article
ms.assetid: 424c06cd-6d61-43cd-a1f2-d1a6b62e47b1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-implement-the-persistent-communications-pattern-using-web-services
msc.type: video
ms.openlocfilehash: 8d7aac37b3b5b47f0533454f2d1d6f1f8677af99
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367479"
---
<a name="how-do-i-implement-the-persistent-communications-pattern-using-web-services"></a><span data-ttu-id="1d136-104">[¿Cómo lo hago?:] ¿Implementar el patrón de comunicaciones persistente mediante servicios Web?</span><span class="sxs-lookup"><span data-stu-id="1d136-104">[How Do I:] Implement the Persistent Communications Pattern using Web Services?</span></span>
====================
<span data-ttu-id="1d136-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1d136-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="1d136-106">En un sitio Web tradicional del explorador y el servidor no mantienen una comunicación continua, pero comunican solo como respuesta al usuario realizar una acción.</span><span class="sxs-lookup"><span data-stu-id="1d136-106">In a traditional Web site the browser and the server do not maintain an ongoing communication, but communicate only in response to the user performing an action.</span></span> <span data-ttu-id="1d136-107">En un sitio Web moderno donde la página se convierte en un contenedor de aplicación, puede ser beneficioso para el explorador y el servidor para mantener una comunicación continua para que puedan realizarse las actualizaciones de página sin que el usuario realiza una acción.</span><span class="sxs-lookup"><span data-stu-id="1d136-107">In a modern Web site where the page becomes an application container, it can be advantageous for the browser and the server to maintain an ongoing communication so that page updates can occur without the user performing an action.</span></span> <span data-ttu-id="1d136-108">Esto se conoce como el patrón de comunicaciones persistente para AJAX.</span><span class="sxs-lookup"><span data-stu-id="1d136-108">This is known as the Persistent Communications Pattern for AJAX.</span></span> <span data-ttu-id="1d136-109">ASP.NET AJAX ofrece dos maneras principales para los desarrolladores Web a implementar el patrón de comunicaciones persistente.</span><span class="sxs-lookup"><span data-stu-id="1d136-109">ASP.NET AJAX provides two main ways for Web developers to implement the Persistent Communications Pattern.</span></span> <span data-ttu-id="1d136-110">En un vídeo anterior, vimos cómo usar ASP.NET AJAX UpdatePanel como base de la implementación.</span><span class="sxs-lookup"><span data-stu-id="1d136-110">In an earlier video we saw how to use the ASP.NET AJAX UpdatePanel as the basis of the implementation.</span></span> <span data-ttu-id="1d136-111">En este vídeo, obtenga información sobre cómo implementar el mismo patrón mediante una llamada JavaScrpt a un servicio Web, lo que elimina la necesidad de un UpdatePanel de ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="1d136-111">In this video we learn how to implement the same pattern using a JavaScrpt call to a Web service, which removes the need for an ASP.NET AJAX UpdatePanel.</span></span>

[<span data-ttu-id="1d136-112">&#9654;Vea el vídeo (16 minutos)</span><span class="sxs-lookup"><span data-stu-id="1d136-112">&#9654; Watch video (16 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-the-persistent-communications-pattern-using-web-services)

> [!div class="step-by-step"]
> <span data-ttu-id="1d136-113">[Anterior](how-do-i-localize-an-aspnet-ajax-application.md)
> [Siguiente](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span><span class="sxs-lookup"><span data-stu-id="1d136-113">[Previous](how-do-i-localize-an-aspnet-ajax-application.md)
[Next](how-do-i-trigger-an-updatepanel-refresh-from-a-dropdownlist-control.md)</span></span>
