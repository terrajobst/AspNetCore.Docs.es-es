---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustar el índice Z de una sombra paralela (VB) | Documentos de Microsoft
author: wenz
description: El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles, para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871303"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="144ad-104">Ajustar el índice Z de una sombra paralela (VB)</span><span class="sxs-lookup"><span data-stu-id="144ad-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="144ad-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="144ad-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="144ad-106">[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="144ad-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="144ad-107">El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="144ad-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="144ad-108">Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="144ad-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="144ad-109">Cuando una entrada de menú aparece, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="144ad-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="144ad-110">Información general</span><span class="sxs-lookup"><span data-stu-id="144ad-110">Overview</span></span>

<span data-ttu-id="144ad-111">El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="144ad-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="144ad-112">Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="144ad-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="144ad-113">Cuando una entrada de menú aparece, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="144ad-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="144ad-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="144ad-114">Steps</span></span>

<span data-ttu-id="144ad-115">El código comienza con el Panel, que contiene texto suficiente como para que el panel contiene texto suficiente para que el efecto sea visible:</span><span class="sxs-lookup"><span data-stu-id="144ad-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="144ad-116">Otro panel se coloca directamente antes de la `panelShadow` panel.</span><span class="sxs-lookup"><span data-stu-id="144ad-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="144ad-117">Contiene un menú con orientación horizontal para que aparezcan las entradas de menú sobre (o en su lugar: en) la `dropShadow` panel):</span><span class="sxs-lookup"><span data-stu-id="144ad-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="144ad-118">A continuación, la `DropShadowExtender` se agrega al ampliar el `panelShadow` panel con un efecto de sombra paralela:</span><span class="sxs-lookup"><span data-stu-id="144ad-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="144ad-119">Por último, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:</span><span class="sxs-lookup"><span data-stu-id="144ad-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="144ad-120">Al ejecutar este script, las entradas de menú aparecen debajo del panel.</span><span class="sxs-lookup"><span data-stu-id="144ad-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="144ad-121">Sin embargo, el menú utiliza la clase CSS `panel` donde sólo tiene que definir dos cosas para que los elementos aparecen delante del otro panel:</span><span class="sxs-lookup"><span data-stu-id="144ad-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="144ad-122">Posición relativa</span><span class="sxs-lookup"><span data-stu-id="144ad-122">Relative positioning</span></span>
- <span data-ttu-id="144ad-123">Un índice de z positivo</span><span class="sxs-lookup"><span data-stu-id="144ad-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="144ad-124">A continuación, la `DropShadowExtender` control no entra en conflicto ya con el control de menú.</span><span class="sxs-lookup"><span data-stu-id="144ad-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="144ad-125">[![Antes: La entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="144ad-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="144ad-126">Antes: La entrada de menú no está visible ([haga clic aquí para ver la imagen a tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="144ad-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="144ad-127">[![Después: La entrada de menú aparece](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="144ad-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="144ad-128">Después: La entrada de menú aparece ([haga clic aquí para ver la imagen a tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="144ad-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="144ad-129">[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Siguiente](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="144ad-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
