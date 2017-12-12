---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: "Animar en respuesta a la interacción del usuario (C#) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Las animaciones pueden estrellas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="eccd5-104">Animar en respuesta a la interacción del usuario (C#)</span><span class="sxs-lookup"><span data-stu-id="eccd5-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="eccd5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eccd5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eccd5-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="eccd5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="eccd5-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="eccd5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eccd5-108">Las animaciones pueden iniciar automáticamente o se pueden desencadenar mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="eccd5-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="eccd5-109">Información general</span><span class="sxs-lookup"><span data-stu-id="eccd5-109">Overview</span></span>

<span data-ttu-id="eccd5-110">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="eccd5-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eccd5-111">Las animaciones pueden iniciar automáticamente o se pueden desencadenar mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="eccd5-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="eccd5-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="eccd5-112">Steps</span></span>

<span data-ttu-id="eccd5-113">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="eccd5-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="eccd5-114">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="eccd5-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="eccd5-115">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="eccd5-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="eccd5-116">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="eccd5-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="eccd5-117">En el `<Animations>` nodo, hay cinco formas de iniciar la animación a través de la interacción del usuario (el elemento que falta es `<OnLoad>` que se ejecuta una vez que se ha cargado completamente toda la página):</span><span class="sxs-lookup"><span data-stu-id="eccd5-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="eccd5-118">`<OnClick>`(el mouse (ratón) haga clic en el control)</span><span class="sxs-lookup"><span data-stu-id="eccd5-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="eccd5-119">`<OnHoverOut>`(mouse deja el control)</span><span class="sxs-lookup"><span data-stu-id="eccd5-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="eccd5-120">`<OnHoverOver>`(se sitúa el mouse sobre un control, detener la `<OnHoverOut>` animación)</span><span class="sxs-lookup"><span data-stu-id="eccd5-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="eccd5-121">`<OnMouseOut>`(el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="eccd5-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="eccd5-122">`<OnMouseOver>`(se sitúa el mouse sobre un control, no se detiene la `<OnMouseOut>` animación)</span><span class="sxs-lookup"><span data-stu-id="eccd5-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="eccd5-123">En este escenario, `<OnClick>` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="eccd5-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="eccd5-124">Cuando el usuario hace clic en el panel, se cambia el tamaño y fundido de salida al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="eccd5-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="eccd5-125">[![La animación inicia un clic del mouse](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eccd5-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="eccd5-126">La animación inicia un clic del mouse ([haga clic aquí para ver la imagen a tamaño completo](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eccd5-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eccd5-127">[Anterior](picking-one-animation-out-of-a-list-cs.md)
[Siguiente](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="eccd5-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
