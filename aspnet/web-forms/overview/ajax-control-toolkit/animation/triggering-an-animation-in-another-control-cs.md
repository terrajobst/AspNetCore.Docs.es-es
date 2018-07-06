---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Desencadenar una animación en otro Control (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Por lo general, iniciar un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810807"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="d93ab-104">Desencadenar una animación en otro Control (C#)</span><span class="sxs-lookup"><span data-stu-id="d93ab-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="d93ab-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d93ab-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d93ab-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d93ab-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="d93ab-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d93ab-108">Por lo general, iniciar una animación se desencadena mediante la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="d93ab-109">Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="d93ab-110">Información general</span><span class="sxs-lookup"><span data-stu-id="d93ab-110">Overview</span></span>

<span data-ttu-id="d93ab-111">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d93ab-112">Por lo general, iniciar una animación se desencadena mediante la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="d93ab-113">Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.</span><span class="sxs-lookup"><span data-stu-id="d93ab-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="d93ab-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="d93ab-114">Steps</span></span>

<span data-ttu-id="d93ab-115">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="d93ab-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="d93ab-116">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="d93ab-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="d93ab-117">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="d93ab-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="d93ab-118">Para iniciar el panel de la animación, se usa un botón HTML.</span><span class="sxs-lookup"><span data-stu-id="d93ab-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="d93ab-119">Tenga en cuenta que `<input type="button" />` es favorecido sobre `<asp:Button />` puesto que no deseamos una devolución de datos cuando el usuario hace clic en ese botón.</span><span class="sxs-lookup"><span data-stu-id="d93ab-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="d93ab-120">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="d93ab-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="d93ab-121">Es importante establecer `TargetControlID` al identificador del botón (el elemento desencadenar la animación), no para el Id. del panel (el elemento que se está animando)</span><span class="sxs-lookup"><span data-stu-id="d93ab-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="d93ab-122">Dentro de la `<Animations>` nodo, las animaciones de contexto como de costumbre.</span><span class="sxs-lookup"><span data-stu-id="d93ab-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="d93ab-123">Para que sean cambiar el panel, no en el botón, establezca la `AnimationTarget` atributo para cada elemento de animación en `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="d93ab-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="d93ab-124">El valor de `AnimationTarget` es el identificador del panel, por supuesto.</span><span class="sxs-lookup"><span data-stu-id="d93ab-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="d93ab-125">De este modo, las animaciones que se producen con el panel, no con el botón de desencadenamiento.</span><span class="sxs-lookup"><span data-stu-id="d93ab-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="d93ab-126">Este es el `AnimationExtender` marcado para este escenario:</span><span class="sxs-lookup"><span data-stu-id="d93ab-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="d93ab-127">Tenga en cuenta el orden en especial en el que aparecen las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="d93ab-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="d93ab-128">En primer lugar, el botón desactiva una vez que se ejecuta la animación.</span><span class="sxs-lookup"><span data-stu-id="d93ab-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="d93ab-129">Dado que no hay ningún `AnimationTarget` atributo el `<EnableAction>` elemento, esta animación se aplica al control de origen: el botón.</span><span class="sxs-lookup"><span data-stu-id="d93ab-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="d93ab-130">Los pasos de la animación en los dos próximos se llevará a cabo parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="d93ab-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="d93ab-131">Ambos tienen sus `AnimationTarget` atributos establecidos en `"Panel1"`, animar, por tanto, el panel, no en el botón.</span><span class="sxs-lookup"><span data-stu-id="d93ab-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="d93ab-132">[![Un clic del mouse en el botón inicia la animación de panel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d93ab-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="d93ab-133">Un clic del mouse en el botón inicia la animación de panel ([haga clic aquí para ver imagen en tamaño completo](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d93ab-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d93ab-134">[Anterior](disabling-actions-during-animation-cs.md)
> [Siguiente](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d93ab-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
