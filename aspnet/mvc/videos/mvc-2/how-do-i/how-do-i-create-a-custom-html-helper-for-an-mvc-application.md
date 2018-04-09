---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: '¿Cómo: crear una aplicación auxiliar de HTML personalizado para una aplicación MVC? | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo crear un HtmlHelper personalizado que no está disponible en el conjunto estándar en una aplicación MVC. En primer lugar, una aplicación MVC de ejemplo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="da3d9-105">¿Cómo: crear una aplicación auxiliar de HTML personalizado para una aplicación MVC?</span><span class="sxs-lookup"><span data-stu-id="da3d9-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="da3d9-106">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="da3d9-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="da3d9-107">En este vídeo Chris Pels muestra cómo crear un HtmlHelper personalizado que no está disponible en el conjunto estándar en una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="da3d9-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="da3d9-108">En primer lugar, se crea una aplicación de MVC de ejemplo con un controlador de demostración y una vista para probar la HtmlHelper personalizado.</span><span class="sxs-lookup"><span data-stu-id="da3d9-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="da3d9-109">A continuación, se crea un módulo con una función pública que sea un método de extensión que representa la implementación de la HtmlHelper personalizado.</span><span class="sxs-lookup"><span data-stu-id="da3d9-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="da3d9-110">La aplicación auxiliar personalizada es para crear `<img>` etiquetas en una página y recibe varios parámetros de entrada, incluidos el identificador, la dirección url y el texto alternativo para la etiqueta de imagen.</span><span class="sxs-lookup"><span data-stu-id="da3d9-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="da3d9-111">La lógica, a continuación, se agrega a la función para devolver el completado `<img>` etiqueta con la información especificada.</span><span class="sxs-lookup"><span data-stu-id="da3d9-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="da3d9-112">A continuación, se utiliza el HtmlHelper personalizado en la página de demostración para mostrar una imagen.</span><span class="sxs-lookup"><span data-stu-id="da3d9-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="da3d9-113">Por último, el HtmlHelper personalizado se expande para incluir varias invalidaciones de constructor que proporcionan flexibilidad para obtener más información crear fácilmente diferentes `<img>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="da3d9-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="da3d9-114">&#9654;Vea el vídeo (18 minutos)</span><span class="sxs-lookup"><span data-stu-id="da3d9-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="da3d9-115">[Anterior](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Siguiente](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="da3d9-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
