---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deshabilite las acciones durante la animación (C#) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También admite la acción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7862c5026a48fbee6eb48beb411e5e1d60c8b406
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870822"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="f3bf7-104">Deshabilite las acciones durante la animación (C#)</span><span class="sxs-lookup"><span data-stu-id="f3bf7-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="f3bf7-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f3bf7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f3bf7-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f3bf7-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="f3bf7-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f3bf7-108">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="f3bf7-109">Sin embargo cuando un clic del mouse, inicia una animación, es deseable deshabilitar clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="f3bf7-110">Información general</span><span class="sxs-lookup"><span data-stu-id="f3bf7-110">Overview</span></span>

<span data-ttu-id="f3bf7-111">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f3bf7-112">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="f3bf7-113">Sin embargo cuando un clic del mouse, inicia una animación, es deseable deshabilitar clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="f3bf7-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="f3bf7-114">Steps</span></span>

<span data-ttu-id="f3bf7-115">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="f3bf7-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="f3bf7-116">La animación se aplicarán a un botón HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f3bf7-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="f3bf7-117">Tenga en cuenta que se utiliza un HTML Control en lugar de un Control Web, puesto que deseamos que el botón para crear un valor devuelto; solo debe iniciar la animación de cliente para que podamos.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="f3bf7-118">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="f3bf7-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="f3bf7-119">En el `<Animations>` nodo, `<OnClick>` es el elemento correcto para controlar el mouse (ratón), haga clic en.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="f3bf7-120">Sin embargo, puede hacer clic en el botón durante la animación, también.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="f3bf7-121">El `<EnableAction>` elemento puede ocuparse de ese.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="f3bf7-122">Establecer `Enabled="false"` deshabilita el botón como parte de la animación.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="f3bf7-123">Puesto que estamos usando varias animaciones individuales (deshabilitar el botón y las animaciones reales), el `<Parallel>` elemento es necesario para pegar las animaciones juntos en una sola.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="f3bf7-124">Este es el marcado completo para `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="f3bf7-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="f3bf7-125">También sería posible volver a habilitar al botón después de la animación, utilizando el elemento XML siguiente al final de la lista:</span><span class="sxs-lookup"><span data-stu-id="f3bf7-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="f3bf7-126">Sin embargo en el escenario que nos ocupa sería inservible desde el botón atenúa y no está visible al final de la animación.</span><span class="sxs-lookup"><span data-stu-id="f3bf7-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="f3bf7-127">[![El botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3bf7-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="f3bf7-128">El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic aquí para ver la imagen a tamaño completo](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f3bf7-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f3bf7-129">[Anterior](animating-in-response-to-user-interaction-cs.md)
> [Siguiente](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f3bf7-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
