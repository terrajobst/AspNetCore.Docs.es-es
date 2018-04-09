---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ejecutar varias animaciones uno tras otro (VB) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="f111f-104">Ejecutar varias animaciones uno tras otro (VB)</span><span class="sxs-lookup"><span data-stu-id="f111f-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="f111f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f111f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f111f-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f111f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="f111f-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="f111f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f111f-108">Permite para ejecutar varias animaciones uno tras otro.</span><span class="sxs-lookup"><span data-stu-id="f111f-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="f111f-109">Información general</span><span class="sxs-lookup"><span data-stu-id="f111f-109">Overview</span></span>

<span data-ttu-id="f111f-110">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="f111f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f111f-111">Permite para ejecutar varias animaciones uno tras otro.</span><span class="sxs-lookup"><span data-stu-id="f111f-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="f111f-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="f111f-112">Steps</span></span>

<span data-ttu-id="f111f-113">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="f111f-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="f111f-114">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="f111f-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="f111f-115">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="f111f-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="f111f-116">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="f111f-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="f111f-117">En el `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="f111f-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="f111f-118">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="f111f-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="f111f-119">El marco de trabajo de animación le permite unir varios animaciones en uno solo mediante la `<Sequence>` elemento.</span><span class="sxs-lookup"><span data-stu-id="f111f-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="f111f-120">Todas las animaciones en `<Sequence>` son ejecutada uno tras otro.</span><span class="sxs-lookup"><span data-stu-id="f111f-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="f111f-121">Aquí es el marcado posibles para el `AnimationExtender` control, en primer lugar hacer que el panel más amplio y, a continuación, disminuir su alto:</span><span class="sxs-lookup"><span data-stu-id="f111f-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="f111f-122">Al ejecutar este script, el panel de primera obtiene más anchos y, a continuación, más pequeños.</span><span class="sxs-lookup"><span data-stu-id="f111f-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="f111f-123">[![En primer lugar se ha aumentado el ancho](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f111f-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="f111f-124">En primer lugar se ha aumentado el ancho ([haga clic aquí para ver la imagen a tamaño completo](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f111f-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="f111f-125">[![A continuación, se reduce el alto](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f111f-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="f111f-126">A continuación, se reduce el alto ([haga clic aquí para ver la imagen a tamaño completo](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f111f-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f111f-127">[Anterior](executing-several-animations-at-the-same-time-vb.md)
> [Siguiente](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f111f-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
