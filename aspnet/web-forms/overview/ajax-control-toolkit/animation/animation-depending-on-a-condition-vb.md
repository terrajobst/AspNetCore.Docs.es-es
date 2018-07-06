---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animación según una condición (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Si una animación es...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b815d7000d94ceec8864b29f0dddf0171344de1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836293"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="21d67-104">Animación según una condición (VB)</span><span class="sxs-lookup"><span data-stu-id="21d67-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="21d67-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="21d67-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="21d67-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="21d67-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="21d67-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="21d67-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="21d67-108">Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21d67-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="21d67-109">Información general</span><span class="sxs-lookup"><span data-stu-id="21d67-109">Overview</span></span>

<span data-ttu-id="21d67-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="21d67-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="21d67-111">Si se ejecuta una animación o no puede también dependen de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="21d67-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="21d67-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="21d67-112">Steps</span></span>

<span data-ttu-id="21d67-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="21d67-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="21d67-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="21d67-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="21d67-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="21d67-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="21d67-116">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="21d67-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="21d67-117">Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="21d67-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="21d67-118">En lugar de una de las animaciones regulares, el `<Condition>` elemento entra en juego.</span><span class="sxs-lookup"><span data-stu-id="21d67-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="21d67-119">El código JavaScript proporcionado como el valor de la `ConditionScript` atributo se ejecuta en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="21d67-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="21d67-120">Si se evalúa como true, la animación se ejecuta, en caso contrario, no.</span><span class="sxs-lookup"><span data-stu-id="21d67-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="21d67-121">El siguiente marcado proporciona dos animaciones, cada uno de ellos que se está ejecutando en el 50% de los casos al azar.</span><span class="sxs-lookup"><span data-stu-id="21d67-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="21d67-122">Puesto que sólo puede haber una animación en `<OnLoad>`, los dos `<Condition>` las animaciones se unen entre sí mediante el `<Sequence>` elemento:</span><span class="sxs-lookup"><span data-stu-id="21d67-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="21d67-123">Tenga en cuenta que el signo menor que (`<`) en el `ConditionScript` atributo debe ser de escape (de).</span><span class="sxs-lookup"><span data-stu-id="21d67-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="21d67-124">Cuando ejecute este script, no hay ejecuciones de animación, o uno de los dos, o ambas lo hacen.</span><span class="sxs-lookup"><span data-stu-id="21d67-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="21d67-125">[![El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="21d67-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="21d67-126">El panel es difuminación sin cambiar el tamaño, por lo que no las segundas ejecuciones de animación, la primera de ellas ([haga clic aquí para ver imagen en tamaño completo](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="21d67-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21d67-127">[Anterior](executing-several-animations-after-each-other-vb.md)
> [Siguiente](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="21d67-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
