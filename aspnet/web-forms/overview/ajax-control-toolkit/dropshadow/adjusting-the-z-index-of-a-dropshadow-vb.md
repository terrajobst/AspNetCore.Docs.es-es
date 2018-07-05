---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustar el índice Z de un control DropShadow (VB) | Microsoft Docs
author: wenz
description: El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela. Sin embargo esta sombra a veces entra en conflicto con otros controles para insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: afc51c6e52a08f46ffc44cc462bdf9a9d5c8ef43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397547"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="12b2d-104">Ajustar el índice Z de un control DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="12b2d-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="12b2d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="12b2d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="12b2d-106">[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="12b2d-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="12b2d-107">El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="12b2d-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="12b2d-108">Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12b2d-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="12b2d-109">Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="12b2d-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="12b2d-110">Información general</span><span class="sxs-lookup"><span data-stu-id="12b2d-110">Overview</span></span>

<span data-ttu-id="12b2d-111">El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="12b2d-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="12b2d-112">Sin embargo esta sombra a veces entra en conflicto con otros controles, por ejemplo el control de menú de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="12b2d-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="12b2d-113">Cuando una entrada de menú emergente, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="12b2d-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="12b2d-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="12b2d-114">Steps</span></span>

<span data-ttu-id="12b2d-115">El código comienza con el Panel, que contiene texto suficiente para que el panel contiene el texto suficiente para que el efecto sea visible:</span><span class="sxs-lookup"><span data-stu-id="12b2d-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="12b2d-116">Otro panel se coloca directamente antes de la `panelShadow` panel.</span><span class="sxs-lookup"><span data-stu-id="12b2d-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="12b2d-117">Contiene un menú con orientación horizontal para que aparezcan entradas de menú sobre (o más bien: en) la `dropShadow` panel):</span><span class="sxs-lookup"><span data-stu-id="12b2d-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="12b2d-118">A continuación, la `DropShadowExtender` se agrega para ampliar el `panelShadow` panel con un efecto de sombra:</span><span class="sxs-lookup"><span data-stu-id="12b2d-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="12b2d-119">Por último, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:</span><span class="sxs-lookup"><span data-stu-id="12b2d-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="12b2d-120">Al ejecutar esta secuencia de comandos, las entradas de menú aparecen debajo del panel.</span><span class="sxs-lookup"><span data-stu-id="12b2d-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="12b2d-121">Sin embargo, el menú usa la clase CSS `panel` donde solo se debe definir dos cosas para hacer que los elementos aparecen delante de otro panel:</span><span class="sxs-lookup"><span data-stu-id="12b2d-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="12b2d-122">Ubicación relativa</span><span class="sxs-lookup"><span data-stu-id="12b2d-122">Relative positioning</span></span>
- <span data-ttu-id="12b2d-123">Un índice z positivo</span><span class="sxs-lookup"><span data-stu-id="12b2d-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="12b2d-124">A continuación, la `DropShadowExtender` control no entra en conflicto ya con el control de menú.</span><span class="sxs-lookup"><span data-stu-id="12b2d-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="12b2d-125">[![Antes: La entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="12b2d-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="12b2d-126">Antes: La entrada de menú no está visible ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="12b2d-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="12b2d-127">[![Después: La entrada de menú aparece](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="12b2d-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="12b2d-128">Después: La entrada de menú aparece ([haga clic aquí para ver imagen en tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="12b2d-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12b2d-129">[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Siguiente](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="12b2d-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
