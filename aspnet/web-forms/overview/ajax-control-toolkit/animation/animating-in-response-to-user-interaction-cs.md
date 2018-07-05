---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animaciones en respuesta a la interacción del usuario (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden star...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9dea9daf3df76558eb19a524475cedd8e2085297
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379824"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="b5c28-104">Animaciones en respuesta a la interacción del usuario (C#)</span><span class="sxs-lookup"><span data-stu-id="b5c28-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="b5c28-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b5c28-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b5c28-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b5c28-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="b5c28-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b5c28-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b5c28-108">Las animaciones pueden iniciar automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="b5c28-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="b5c28-109">Información general</span><span class="sxs-lookup"><span data-stu-id="b5c28-109">Overview</span></span>

<span data-ttu-id="b5c28-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="b5c28-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b5c28-111">Las animaciones pueden iniciar automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="b5c28-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="b5c28-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="b5c28-112">Steps</span></span>

<span data-ttu-id="b5c28-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="b5c28-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="b5c28-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="b5c28-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="b5c28-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="b5c28-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="b5c28-116">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="b5c28-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="b5c28-117">Dentro de la `<Animations>` nodo, existen cinco formas de iniciar la animación a través de la interacción del usuario (el elemento que falta es `<OnLoad>` que se ejecuta una vez que se ha cargado completamente toda la página):</span><span class="sxs-lookup"><span data-stu-id="b5c28-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="b5c28-118">`<OnClick>` (clic del mouse en el control)</span><span class="sxs-lookup"><span data-stu-id="b5c28-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="b5c28-119">`<OnHoverOut>` (mouse deja el control)</span><span class="sxs-lookup"><span data-stu-id="b5c28-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="b5c28-120">`<OnHoverOver>` (se mantiene el mouse sobre un control, deteniendo el `<OnHoverOut>` animación)</span><span class="sxs-lookup"><span data-stu-id="b5c28-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="b5c28-121">`<OnMouseOut>` (el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="b5c28-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="b5c28-122">`<OnMouseOver>` (se mantiene el mouse sobre un control, no Deteniendo el `<OnMouseOut>` animación)</span><span class="sxs-lookup"><span data-stu-id="b5c28-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="b5c28-123">En este escenario, `<OnClick>` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="b5c28-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="b5c28-124">Cuando el usuario hace clic en el panel, se cambia el tamaño y atenúa al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="b5c28-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="b5c28-125">[![La animación inicia un clic del mouse](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b5c28-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="b5c28-126">La animación inicia un clic del mouse ([haga clic aquí para ver imagen en tamaño completo](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b5c28-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b5c28-127">[Anterior](picking-one-animation-out-of-a-list-cs.md)
> [Siguiente](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b5c28-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
