---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Enlace de datos el Control deslizante (C#) | Microsoft Docs
author: wenz
description: El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible enlazar la sición actual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 951a7484f0dbb14ee7f1e1d62c9666e5cc49e7c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837859"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="1ce37-104">Enlace de datos el Control deslizante (C#)</span><span class="sxs-lookup"><span data-stu-id="1ce37-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="1ce37-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1ce37-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1ce37-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1ce37-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="1ce37-107">El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse.</span><span class="sxs-lookup"><span data-stu-id="1ce37-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1ce37-108">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ce37-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="1ce37-109">Información general</span><span class="sxs-lookup"><span data-stu-id="1ce37-109">Overview</span></span>

<span data-ttu-id="1ce37-110">El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse.</span><span class="sxs-lookup"><span data-stu-id="1ce37-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="1ce37-111">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1ce37-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="1ce37-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="1ce37-112">Steps</span></span>

<span data-ttu-id="1ce37-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="1ce37-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="1ce37-114">A continuación, agregue dos `TextBox` controles a la página.</span><span class="sxs-lookup"><span data-stu-id="1ce37-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="1ce37-115">Uno se transformará en un control deslizante gráfico y el otro va a contener la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="1ce37-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="1ce37-116">El siguiente paso es ya el paso final.</span><span class="sxs-lookup"><span data-stu-id="1ce37-116">The next step is already the final step.</span></span> <span data-ttu-id="1ce37-117">El `SliderExtender` control de ASP.NET AJAX Control Toolkit hace que un control deslizante del primer cuadro de texto y el segundo cuadro de texto se actualiza automáticamente cuando el control deslizante colocar los cambios.</span><span class="sxs-lookup"><span data-stu-id="1ce37-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="1ce37-118">En orden para que funcione, el `SliderExtender`del `TargetControlID` atributo debe establecerse en el identificador del primer cuadro de texto; el `BoundControlID` atributo debe establecerse en el identificador del segundo cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="1ce37-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="1ce37-119">Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: escribir un nuevo valor en el cuadro de texto actualiza la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="1ce37-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="1ce37-120">Si realiza el segundo cuadro de texto de solo lectura, puede agregar una protección no segura para el campo de texto para que resulte más difícil para el usuario actualizar manualmente el valor allí.</span><span class="sxs-lookup"><span data-stu-id="1ce37-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="1ce37-121">[![Cuadro de texto y el control deslizante estén sincronizados](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1ce37-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="1ce37-122">Cuadro de texto y el control deslizante estén sincronizados ([haga clic aquí para ver imagen en tamaño completo](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1ce37-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ce37-123">[Anterior](using-the-slider-control-with-auto-postback-cs.md)
> [Siguiente](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1ce37-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
