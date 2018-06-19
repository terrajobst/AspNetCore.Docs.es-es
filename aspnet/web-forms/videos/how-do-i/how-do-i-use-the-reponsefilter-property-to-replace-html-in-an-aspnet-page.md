---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[¿Cómo I:] Utilice la propiedad Reponse.Filter para reemplazar el código HTML en una página ASP.NET | Documentos de Microsoft'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo utilizar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página. En primer lugar, se crea una página de ejemplo w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525534"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="23c44-104">[¿Cómo I:] Use la propiedad Reponse.Filter para reemplazar el código HTML en una página ASP.NET</span><span class="sxs-lookup"><span data-stu-id="23c44-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="23c44-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="23c44-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="23c44-106">En este vídeo Chris Pels muestra cómo utilizar la propiedad Reponse.Filter para interceptar y modificar el código HTML que se envían a una página.</span><span class="sxs-lookup"><span data-stu-id="23c44-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="23c44-107">En primer lugar, se crea una página de ejemplo con algún texto simple.</span><span class="sxs-lookup"><span data-stu-id="23c44-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="23c44-108">A continuación, se crea una clase de secuencia personalizada que actúa como la secuencia de reemplazo de la secuencia actual que se envían al explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="23c44-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="23c44-109">En esa clase de secuencia personalizada se recupera el contenido de la página de la secuencia, modifica y, a continuación, escribe en la secuencia de respuesta.</span><span class="sxs-lookup"><span data-stu-id="23c44-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="23c44-110">En esta clase de secuencia personalizada al método de escritura se ha personalizado para reemplazar el código HTML en la secuencia de respuesta base, con lo que modificar lo que se envía al explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="23c44-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="23c44-111">Por último, la nueva clase de secuencia se asigna a la propiedad en la página\_cargar eventos, por lo tanto, proporciona el mecanismo para modificar el contenido de la página.</span><span class="sxs-lookup"><span data-stu-id="23c44-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="23c44-112">&#9654; Vea el vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="23c44-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
