---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Ejecutar varias animaciones al mismo tiempo (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Lo que permite para ejecutar severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: d70f7b9170cbd3307dae4cdb4f9ee735e3c5bee8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829733"
---
<a name="executing-several-animations-at-the-same-time-c"></a><span data-ttu-id="2ff26-104">Ejecutar varias animaciones al mismo tiempo (C#)</span><span class="sxs-lookup"><span data-stu-id="2ff26-104">Executing Several Animations at The Same Time (C#)</span></span>
====================
<span data-ttu-id="2ff26-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ff26-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ff26-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ff26-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)</span></span>

> <span data-ttu-id="2ff26-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="2ff26-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ff26-108">Lo que permite para ejecutar varias animaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="2ff26-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="2ff26-109">Información general</span><span class="sxs-lookup"><span data-stu-id="2ff26-109">Overview</span></span>

<span data-ttu-id="2ff26-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="2ff26-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ff26-111">Lo que permite para ejecutar varias animaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="2ff26-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="2ff26-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="2ff26-112">Steps</span></span>

<span data-ttu-id="2ff26-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="2ff26-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

<span data-ttu-id="2ff26-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="2ff26-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

<span data-ttu-id="2ff26-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="2ff26-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

<span data-ttu-id="2ff26-116">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="2ff26-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

<span data-ttu-id="2ff26-117">Dentro de la `<Animations>` nodo, use `<OnLoad>` para ejecutar las animaciones de una vez que la página se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="2ff26-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2ff26-118">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="2ff26-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="2ff26-119">El marco de animación le permite unir varias animaciones en uno solo mediante la `<Parallel>` elemento.</span><span class="sxs-lookup"><span data-stu-id="2ff26-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="2ff26-120">Todas las animaciones en `<Parallel>` se ejecutan al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="2ff26-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="2ff26-121">Este es el un marcado posible para el `AnimationExtender` control difuminación y el tamaño del panel al mismo tiempo:</span><span class="sxs-lookup"><span data-stu-id="2ff26-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

<span data-ttu-id="2ff26-122">Y realmente: al ejecutar este script, el panel se muestra, a continuación, se cambia el tamaño (más que threefolding su ancho y halfing su alto) y el fundido de salida al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="2ff26-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="2ff26-123">[![El panel se atenúa y cambiar el tamaño (incluido su contenido, gracias al motor de representación del explorador)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ff26-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)</span></span>

<span data-ttu-id="2ff26-124">El panel se atenúa y cambiar el tamaño (incluido su contenido, gracias al motor de representación del explorador) ([haga clic aquí para ver imagen en tamaño completo](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ff26-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ff26-125">[Anterior](adding-animation-to-a-control-cs.md)
> [Siguiente](executing-several-animations-after-each-other-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2ff26-125">[Previous](adding-animation-to-a-control-cs.md)
[Next](executing-several-animations-after-each-other-cs.md)</span></span>
