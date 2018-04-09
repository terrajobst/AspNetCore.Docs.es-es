---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Agregar dinámicamente un panel acordeón (C#) | Documentos de Microsoft
author: wenz
description: El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="197a6-104">Agregar dinámicamente un panel acordeón (C#)</span><span class="sxs-lookup"><span data-stu-id="197a6-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="197a6-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="197a6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="197a6-106">[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="197a6-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="197a6-107">El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="197a6-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="197a6-108">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="197a6-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="197a6-109">Información general</span><span class="sxs-lookup"><span data-stu-id="197a6-109">Overview</span></span>

<span data-ttu-id="197a6-110">El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="197a6-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="197a6-111">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="197a6-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="197a6-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="197a6-112">Steps</span></span>

<span data-ttu-id="197a6-113">El control Accordion expone todas las propiedades importantes para el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="197a6-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="197a6-114">Entre otras cosas, la `Panes` propiedad concede acceso a la colección de paneles que forman el Accordion.</span><span class="sxs-lookup"><span data-stu-id="197a6-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="197a6-115">Cada panel hay de tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="197a6-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="197a6-116">Por lo tanto, es muy fácil de crear un panel de este tipo:</span><span class="sxs-lookup"><span data-stu-id="197a6-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="197a6-117">El `HeaderContainer` propiedad de `AccordionPane` proporciona acceso a los controles ASP.NET en la sección de encabezado del panel; la `ContentContainer` propiedad de `AccordionPane` hace lo mismo para la sección de contenido del panel.</span><span class="sxs-lookup"><span data-stu-id="197a6-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="197a6-118">Esto permite que el código ASP.NET agregar contenido a los paneles:</span><span class="sxs-lookup"><span data-stu-id="197a6-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="197a6-119">Por último, se deben agregar el panel o paneles a la `Panes` colección de la Accordion:</span><span class="sxs-lookup"><span data-stu-id="197a6-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="197a6-120">Este es un código de servidor completo que agrega dos paneles a un control Accordion:</span><span class="sxs-lookup"><span data-stu-id="197a6-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="197a6-121">El único elemento que falta es la Accordion, que depende de la presencia de ASP.NET `ScriptManager` control:</span><span class="sxs-lookup"><span data-stu-id="197a6-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="197a6-122">Para terminar el ejemplo, las dos clases CSS que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:</span><span class="sxs-lookup"><span data-stu-id="197a6-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="197a6-123">[![Los datos de la accordion dinámicamente se agregan mediante código del lado servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="197a6-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="197a6-124">Los datos de la accordion dinámicamente se agregan mediante código del lado servidor ([haga clic aquí para ver la imagen a tamaño completo](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="197a6-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="197a6-125">[Anterior](databinding-to-an-accordion-cs.md)
> [Siguiente](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="197a6-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
