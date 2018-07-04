---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Agregar animación a un Control (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial se muestra cómo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3909422dc5d261b39f3efd7d7eaeb5cfb1976f2b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364931"
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="4dbfc-104">Agregar animación a un Control (VB)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="4dbfc-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4dbfc-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="4dbfc-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4dbfc-108">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="4dbfc-109">Información general</span><span class="sxs-lookup"><span data-stu-id="4dbfc-109">Overview</span></span>

<span data-ttu-id="4dbfc-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4dbfc-111">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="4dbfc-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="4dbfc-112">Steps</span></span>

<span data-ttu-id="4dbfc-113">El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="4dbfc-114">La animación en este escenario se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="4dbfc-115">La clase CSS asociada para el panel define un ancho y un color de fondo:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="4dbfc-116">A continuación, necesitamos el `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="4dbfc-117">Después de proporcionar un `ID` y el habitual `runat="server"`, el `TargetControlID` atributo debe establecerse para el control se va a animar en nuestro caso, el panel:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="4dbfc-118">Mediante declaración, se aplica la animación completa mediante una sintaxis XML, lamentablemente actualmente no admitida totalmente IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="4dbfc-119">El nodo raíz es `<Animations>;` dentro de este nodo, se permiten varios eventos que determinan cuándo la animación adopten lugar:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="4dbfc-120">`OnClick` (clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="4dbfc-121">`OnHoverOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="4dbfc-122">`OnHoverOver` (al mantener el mouse sobre un control, deteniendo el `OnHoverOut` animación)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="4dbfc-123">`OnLoad` (cuando se ha cargado la página)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="4dbfc-124">`OnMouseOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="4dbfc-125">`OnMouseOver` (al mantener el mouse sobre un control, no Deteniendo el `OnMouseOut` animación)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="4dbfc-126">El marco de trabajo incluye un conjunto de animaciones, cada uno representado por su propio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="4dbfc-127">Aquí es una selección:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-127">Here is a selection:</span></span>

- <span data-ttu-id="4dbfc-128">`<Color>` (cambiar un color)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="4dbfc-129">`<FadeIn>` (resaltar)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="4dbfc-130">`<FadeOut>` (difuminado)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="4dbfc-131">`<Property>` (cambiar la propiedad de un control)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="4dbfc-132">`<Pulse>` (en alza)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="4dbfc-133">`<Resize>` (cambiar el tamaño)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="4dbfc-134">`<Scale>` (cambiar proporcionalmente el tamaño)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="4dbfc-135">En este ejemplo, el panel será fundido de salida. La animación adoptarán 1,5 segundos (`Duration` atributo), mostrar (pasos de animación) de 24 fotogramas por segundo (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="4dbfc-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="4dbfc-136">Aquí está el marcado completo para el `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="4dbfc-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="4dbfc-137">Al ejecutar este script, el panel se muestra y atenúa en segundos de uno y medio.</span><span class="sxs-lookup"><span data-stu-id="4dbfc-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="4dbfc-138">[![El panel se atenúa](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="4dbfc-139">El panel se atenúa ([haga clic aquí para ver imagen en tamaño completo](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4dbfc-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4dbfc-140">[Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Siguiente](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4dbfc-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
