---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animación según una condición (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Si es una animación...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="546f5-104">Animación según una condición (VB)</span><span class="sxs-lookup"><span data-stu-id="546f5-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="546f5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="546f5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="546f5-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="546f5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="546f5-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="546f5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="546f5-108">Si se ejecuta una animación o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="546f5-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="546f5-109">Información general</span><span class="sxs-lookup"><span data-stu-id="546f5-109">Overview</span></span>

<span data-ttu-id="546f5-110">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="546f5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="546f5-111">Si se ejecuta una animación o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="546f5-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="546f5-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="546f5-112">Steps</span></span>

<span data-ttu-id="546f5-113">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="546f5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="546f5-114">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="546f5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="546f5-115">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="546f5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="546f5-116">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="546f5-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="546f5-117">En el `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="546f5-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="546f5-118">En lugar de una de las animaciones regulares, el `<Condition>` elemento entra en juego.</span><span class="sxs-lookup"><span data-stu-id="546f5-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="546f5-119">El código de JavaScript que se proporciona como el valor de la `ConditionScript` atributo se ejecuta en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="546f5-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="546f5-120">Si se evalúa como true, la animación se ejecuta, en caso contrario, no.</span><span class="sxs-lookup"><span data-stu-id="546f5-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="546f5-121">El siguiente marcado proporciona dos animaciones, cada uno de ellos que se está ejecutando en el 50% de los casos en aleatorio.</span><span class="sxs-lookup"><span data-stu-id="546f5-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="546f5-122">Dado que solo puede haber una animación en `<OnLoad>`, los dos `<Condition>` animaciones se combinan entre sí mediante el `<Sequence>` elemento:</span><span class="sxs-lookup"><span data-stu-id="546f5-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="546f5-123">Tenga en cuenta que el signo menor que (`<`) en el `ConditionScript` atributo debe ser de escape ().</span><span class="sxs-lookup"><span data-stu-id="546f5-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="546f5-124">Al ejecutar este script, no hay ejecuciones de animación, o realiza una de las dos o ninguna lo hace.</span><span class="sxs-lookup"><span data-stu-id="546f5-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="546f5-125">[![El panel se atenúa, sin cambiar el tamaño, por lo que el segundo ejecuciones de animación, la primera de ellas no](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="546f5-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="546f5-126">El panel se atenúa, sin cambiar el tamaño, por lo que el segundo ejecuciones de animación, la primera de ellas no ([haga clic aquí para ver la imagen a tamaño completo](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="546f5-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="546f5-127">[Anterior](executing-several-animations-after-each-other-vb.md)
> [Siguiente](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="546f5-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
