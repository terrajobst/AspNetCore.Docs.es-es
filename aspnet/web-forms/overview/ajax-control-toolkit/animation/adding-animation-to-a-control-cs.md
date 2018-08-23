---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Agregar animación a un Control (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836582"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="ce1a3-104">Agregar animación a un Control (C#)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="ce1a3-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce1a3-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="ce1a3-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce1a3-108">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="ce1a3-109">Información general</span><span class="sxs-lookup"><span data-stu-id="ce1a3-109">Overview</span></span>

<span data-ttu-id="ce1a3-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce1a3-111">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ce1a3-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="ce1a3-112">Steps</span></span>

<span data-ttu-id="ce1a3-113">El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ce1a3-114">La animación en este escenario se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ce1a3-115">La clase CSS asociada para el panel define un ancho y un color de fondo:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="ce1a3-116">A continuación, necesitamos el `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ce1a3-117">Después de proporcionar un `ID` y el habitual `runat="server"`, el `TargetControlID` atributo debe establecerse para el control se va a animar en nuestro caso, el panel:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ce1a3-118">Mediante declaración, se aplica la animación completa mediante una sintaxis XML, lamentablemente actualmente no admitida totalmente IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ce1a3-119">El nodo raíz es `<Animations>;` dentro de este nodo, se permiten varios eventos que determinan cuándo la animación adopten lugar:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ce1a3-120">`OnClick` (clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ce1a3-121">`OnHoverOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ce1a3-122">`OnHoverOver` (al mantener el mouse sobre un control, deteniendo el `OnHoverOut` animación)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ce1a3-123">`OnLoad` (cuando se ha cargado la página)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ce1a3-124">`OnMouseOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ce1a3-125">`OnMouseOver` (al mantener el mouse sobre un control, no Deteniendo el `OnMouseOut` animación)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ce1a3-126">El marco de trabajo incluye un conjunto de animaciones, cada uno representado por su propio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ce1a3-127">Aquí es una selección:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-127">Here is a selection:</span></span>

- <span data-ttu-id="ce1a3-128">`<Color>` (cambiar un color)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ce1a3-129">`<FadeIn>` (resaltar)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ce1a3-130">`<FadeOut>` (difuminado)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ce1a3-131">`<Property>` (cambiar la propiedad de un control)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ce1a3-132">`<Pulse>` (en alza)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ce1a3-133">`<Resize>` (cambiar el tamaño)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ce1a3-134">`<Scale>` (cambiar proporcionalmente el tamaño)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ce1a3-135">En este ejemplo, el panel será fundido de salida. La animación adoptarán 1,5 segundos (`Duration` atributo), mostrar (pasos de animación) de 24 fotogramas por segundo (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="ce1a3-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="ce1a3-136">Aquí está el marcado completo para el `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="ce1a3-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ce1a3-137">Al ejecutar este script, el panel se muestra y atenúa en segundos de uno y medio.</span><span class="sxs-lookup"><span data-stu-id="ce1a3-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="ce1a3-138">[![El panel se atenúa](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce1a3-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ce1a3-139">El panel se atenúa ([haga clic aquí para ver imagen en tamaño completo](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce1a3-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce1a3-140">Siguiente</span><span class="sxs-lookup"><span data-stu-id="ce1a3-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
