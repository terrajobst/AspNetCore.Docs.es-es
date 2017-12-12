---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: "Agregar dinámicamente un panel acordeón (VB) | Documentos de Microsoft"
author: wenz
description: El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adc0b7985e5ba3da1684b645ae1b71b5112594a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="64fae-104">Agregar dinámicamente un panel acordeón (VB)</span><span class="sxs-lookup"><span data-stu-id="64fae-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="64fae-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="64fae-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="64fae-106">[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="64fae-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="64fae-107">El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="64fae-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="64fae-108">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="64fae-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="64fae-109">Información general</span><span class="sxs-lookup"><span data-stu-id="64fae-109">Overview</span></span>

<span data-ttu-id="64fae-110">El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="64fae-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="64fae-111">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="64fae-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="64fae-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="64fae-112">Steps</span></span>

<span data-ttu-id="64fae-113">El control Accordion expone todas las propiedades importantes para el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="64fae-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="64fae-114">Entre otras cosas, la `Panes` propiedad concede acceso a la colección de paneles que forman el Accordion.</span><span class="sxs-lookup"><span data-stu-id="64fae-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="64fae-115">Cada panel hay de tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="64fae-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="64fae-116">Por lo tanto, es muy fácil de crear un panel de este tipo:</span><span class="sxs-lookup"><span data-stu-id="64fae-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="64fae-117">El `HeaderContainer` propiedad de `AccordionPane` proporciona acceso a los controles ASP.NET en la sección de encabezado del panel; la `ContentContainer` propiedad de `AccordionPane` hace lo mismo para la sección de contenido del panel.</span><span class="sxs-lookup"><span data-stu-id="64fae-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="64fae-118">Esto permite que el código ASP.NET agregar contenido a los paneles:</span><span class="sxs-lookup"><span data-stu-id="64fae-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="64fae-119">Por último, se deben agregar el panel o paneles a la `Panes` colección de la Accordion:</span><span class="sxs-lookup"><span data-stu-id="64fae-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="64fae-120">Este es un código de servidor completo que agrega dos paneles a un control Accordion:</span><span class="sxs-lookup"><span data-stu-id="64fae-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="64fae-121">El único elemento que falta es la Accordion, que depende de la presencia de ASP.NET `ScriptManager` control:</span><span class="sxs-lookup"><span data-stu-id="64fae-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="64fae-122">Para terminar el ejemplo, las dos clases CSS que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:</span><span class="sxs-lookup"><span data-stu-id="64fae-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="64fae-123">[![Los datos de la accordion dinámicamente se agregan mediante código del lado servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="64fae-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="64fae-124">Los datos de la accordion dinámicamente se agregan mediante código del lado servidor ([haga clic aquí para ver la imagen a tamaño completo](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="64fae-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="64fae-125">Anterior</span><span class="sxs-lookup"><span data-stu-id="64fae-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
