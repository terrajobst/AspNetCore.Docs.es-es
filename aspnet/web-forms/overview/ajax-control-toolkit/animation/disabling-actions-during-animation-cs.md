---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deshabilitar las acciones durante la animación (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. También admite la acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: a82d46f47cf12b29284bf9211545f8984a586c03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365586"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="e487d-104">Deshabilitar las acciones durante la animación (C#)</span><span class="sxs-lookup"><span data-stu-id="e487d-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="e487d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e487d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e487d-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e487d-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="e487d-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="e487d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e487d-108">También es compatible con las acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="e487d-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="e487d-109">Sin embargo cuando un clic del mouse inicia una animación, es deseable deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="e487d-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="e487d-110">Información general</span><span class="sxs-lookup"><span data-stu-id="e487d-110">Overview</span></span>

<span data-ttu-id="e487d-111">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="e487d-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e487d-112">También es compatible con las acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="e487d-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="e487d-113">Sin embargo cuando un clic del mouse inicia una animación, es deseable deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="e487d-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="e487d-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="e487d-114">Steps</span></span>

<span data-ttu-id="e487d-115">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="e487d-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="e487d-116">La animación se aplicarán a un botón HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="e487d-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="e487d-117">Tenga en cuenta que, puesto que deseamos que el botón para crear un postback; se usa un HTML Control en lugar de un Control Web solo deberá iniciar la animación del lado cliente para nosotros.</span><span class="sxs-lookup"><span data-stu-id="e487d-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="e487d-118">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="e487d-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="e487d-119">Dentro de la `<Animations>` nodo, `<OnClick>` es el elemento correcto para controlar el clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="e487d-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="e487d-120">Sin embargo, se puede hacer clic en el botón durante la animación, también.</span><span class="sxs-lookup"><span data-stu-id="e487d-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="e487d-121">El `<EnableAction>` elemento puede ocuparse de eso.</span><span class="sxs-lookup"><span data-stu-id="e487d-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="e487d-122">Establecer `Enabled="false"` deshabilita el botón como parte de la animación.</span><span class="sxs-lookup"><span data-stu-id="e487d-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="e487d-123">Puesto que estamos usando varias animaciones individuales (deshabilitar el botón y las animaciones reales), el `<Parallel>` elemento tiene que pegar las animaciones juntos en una sola.</span><span class="sxs-lookup"><span data-stu-id="e487d-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="e487d-124">Aquí está el marcado completo para `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="e487d-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="e487d-125">También sería posible volver a habilitar al botón después de la animación, mediante el siguiente elemento XML al final de la lista:</span><span class="sxs-lookup"><span data-stu-id="e487d-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="e487d-126">Sin embargo en un escenario determinado sería inútil desde el botón se atenúa y no está visible al final de la animación.</span><span class="sxs-lookup"><span data-stu-id="e487d-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="e487d-127">[![El botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e487d-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="e487d-128">El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic aquí para ver imagen en tamaño completo](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e487d-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e487d-129">[Anterior](animating-in-response-to-user-interaction-cs.md)
> [Siguiente](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e487d-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
