---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Agregar dinámicamente un panel Accordion (VB) | Microsoft Docs
author: wenz
description: El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820031"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="eebf1-104">Agregar dinámicamente un panel Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="eebf1-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="eebf1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eebf1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eebf1-106">[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="eebf1-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="eebf1-107">El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="eebf1-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="eebf1-108">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="eebf1-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="eebf1-109">Información general</span><span class="sxs-lookup"><span data-stu-id="eebf1-109">Overview</span></span>

<span data-ttu-id="eebf1-110">El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="eebf1-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="eebf1-111">Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.</span><span class="sxs-lookup"><span data-stu-id="eebf1-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="eebf1-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="eebf1-112">Steps</span></span>

<span data-ttu-id="eebf1-113">El control Accordion expone todas las propiedades importantes de código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="eebf1-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="eebf1-114">Entre otras cosas, la `Panes` propiedad concede acceso a la colección de paneles que componen el Accordion.</span><span class="sxs-lookup"><span data-stu-id="eebf1-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="eebf1-115">Cada panel hay de tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="eebf1-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="eebf1-116">Por lo tanto, es muy fácil crear un panel de este tipo:</span><span class="sxs-lookup"><span data-stu-id="eebf1-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="eebf1-117">El `HeaderContainer` propiedad de `AccordionPane` proporciona acceso a los controles de ASP.NET dentro de la sección de encabezado del panel; la `ContentContainer` propiedad de `AccordionPane` hace lo mismo para la sección contenido del panel.</span><span class="sxs-lookup"><span data-stu-id="eebf1-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="eebf1-118">Esto permite que el código ASP.NET agregar contenido a los paneles:</span><span class="sxs-lookup"><span data-stu-id="eebf1-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="eebf1-119">Por último, el panel o paneles deben agregarse a la `Panes` colección de lo Accordion:</span><span class="sxs-lookup"><span data-stu-id="eebf1-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="eebf1-120">Este es un código de servidor completando que agrega dos paneles a un control Accordion:</span><span class="sxs-lookup"><span data-stu-id="eebf1-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="eebf1-121">El único elemento que falta es el propio, acordeón que depende de la presencia de ASP.NET `ScriptManager` control:</span><span class="sxs-lookup"><span data-stu-id="eebf1-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="eebf1-122">Para finalizar el ejemplo, las dos clases CSS que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:</span><span class="sxs-lookup"><span data-stu-id="eebf1-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="eebf1-123">[![Los datos en el accordion dinámicamente se agregan mediante código de servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eebf1-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="eebf1-124">Los datos en el accordion dinámicamente se agregan mediante código de servidor ([haga clic aquí para ver imagen en tamaño completo](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eebf1-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="eebf1-125">Anterior</span><span class="sxs-lookup"><span data-stu-id="eebf1-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
