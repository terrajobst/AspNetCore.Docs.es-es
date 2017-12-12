---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "Desencadenar una animación en otro Control (VB) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Por lo general, iniciar un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="ce29b-104">Desencadenar una animación en otro Control (VB)</span><span class="sxs-lookup"><span data-stu-id="ce29b-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="ce29b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce29b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce29b-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce29b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="ce29b-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce29b-108">Por lo general, iniciar una animación desencadenan la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="ce29b-109">Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="ce29b-110">Información general</span><span class="sxs-lookup"><span data-stu-id="ce29b-110">Overview</span></span>

<span data-ttu-id="ce29b-111">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce29b-112">Por lo general, iniciar una animación desencadenan la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="ce29b-113">Sin embargo, también es posible interactuar con un control y, a continuación, la animación de otro control.</span><span class="sxs-lookup"><span data-stu-id="ce29b-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="ce29b-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="ce29b-114">Steps</span></span>

<span data-ttu-id="ce29b-115">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="ce29b-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="ce29b-116">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="ce29b-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="ce29b-117">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="ce29b-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="ce29b-118">Para iniciar el panel de la animación, se usa un botón HTML.</span><span class="sxs-lookup"><span data-stu-id="ce29b-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="ce29b-119">Tenga en cuenta que `<input type="button" />` favorecido sobre `<asp:Button />` puesto que no deseamos una devolución de datos cuando el usuario hace clic en ese botón.</span><span class="sxs-lookup"><span data-stu-id="ce29b-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="ce29b-120">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="ce29b-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="ce29b-121">Es importante establecer `TargetControlID` con el identificador del botón (el elemento desencadenar la animación), no para el Id. del panel (es decir, el elemento que se está animando)</span><span class="sxs-lookup"><span data-stu-id="ce29b-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="ce29b-122">En el `<Animations>` nodo, animaciones de contexto como de costumbre.</span><span class="sxs-lookup"><span data-stu-id="ce29b-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="ce29b-123">Para que pueda cambiar el panel, no en el botón, establezca la `AnimationTarget` atributo para cada elemento de animación en `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="ce29b-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="ce29b-124">El valor de `AnimationTarget` es el identificador del panel, por supuesto.</span><span class="sxs-lookup"><span data-stu-id="ce29b-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="ce29b-125">De este modo, las animaciones suceder con el panel, no con el botón de activación.</span><span class="sxs-lookup"><span data-stu-id="ce29b-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="ce29b-126">Este es el `AnimationExtender` marcado para este escenario:</span><span class="sxs-lookup"><span data-stu-id="ce29b-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="ce29b-127">Tenga en cuenta el orden especial en el que aparecen las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="ce29b-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="ce29b-128">En primer lugar, el botón obtiene desactiva una vez que se ejecuta la animación.</span><span class="sxs-lookup"><span data-stu-id="ce29b-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="ce29b-129">Dado que no hay ningún `AnimationTarget` de atributo en el `<EnableAction>` elemento, esta animación se aplica al control de origen: el botón.</span><span class="sxs-lookup"><span data-stu-id="ce29b-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="ce29b-130">Los pasos siguientes dos animación se llevará a cabo parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="ce29b-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="ce29b-131">Ambos tienen sus `AnimationTarget` atributos establecidos en `"Panel1"`, animación, por tanto, el panel, no en el botón.</span><span class="sxs-lookup"><span data-stu-id="ce29b-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="ce29b-132">[![Un clic del mouse en el botón inicia la animación de panel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce29b-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="ce29b-133">Un clic del mouse en el botón inicia la animación de panel ([haga clic aquí para ver la imagen a tamaño completo](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce29b-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ce29b-134">[Anterior](disabling-actions-during-animation-vb.md)
[Siguiente](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ce29b-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
