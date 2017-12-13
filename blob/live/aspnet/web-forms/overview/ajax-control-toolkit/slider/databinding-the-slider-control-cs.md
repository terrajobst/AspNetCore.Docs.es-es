---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Enlace de datos del Control deslizante (C#) | Documentos de Microsoft
author: wenz
description: "El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse. Es posible enlazar el sición actual..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2aa770bce5969a4ab57893d5ceecc127cf7f7872
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="e6a59-104">Enlace de datos del Control deslizante (C#)</span><span class="sxs-lookup"><span data-stu-id="e6a59-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="e6a59-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e6a59-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e6a59-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e6a59-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="e6a59-107">El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="e6a59-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e6a59-108">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6a59-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="e6a59-109">Información general</span><span class="sxs-lookup"><span data-stu-id="e6a59-109">Overview</span></span>

<span data-ttu-id="e6a59-110">El control deslizante en el Kit de herramientas de Control de AJAX proporciona un control deslizante gráfico que puede controlarse mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="e6a59-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="e6a59-111">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6a59-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="e6a59-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="e6a59-112">Steps</span></span>

<span data-ttu-id="e6a59-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="e6a59-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="e6a59-114">A continuación, agregue dos `TextBox` controles a la página.</span><span class="sxs-lookup"><span data-stu-id="e6a59-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="e6a59-115">Uno se transformará en un control deslizante gráfico y el otro se mantendrá la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="e6a59-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="e6a59-116">El siguiente paso ya es el último paso.</span><span class="sxs-lookup"><span data-stu-id="e6a59-116">The next step is already the final step.</span></span> <span data-ttu-id="e6a59-117">El `SliderExtender` control desde el Kit de herramientas de Control de AJAX de ASP.NET realiza un control deslizante del primer cuadro de texto y el segundo cuadro de texto se actualiza automáticamente cuando el control deslizante posición cambios.</span><span class="sxs-lookup"><span data-stu-id="e6a59-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="e6a59-118">En orden para que esto funcione, el `SliderExtender`del `TargetControlID` atributo debe establecerse en el identificador del primer cuadro de texto; el `BoundControlID` atributo debe establecerse en el identificador del segundo cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="e6a59-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="e6a59-119">Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: escribir un nuevo valor en el cuadro de texto actualiza la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="e6a59-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="e6a59-120">Si realiza el segundo cuadro de texto de solo lectura, puede agregar una protección no segura para el campo de texto para que resulte más difícil para el usuario actualizar manualmente el valor de allí.</span><span class="sxs-lookup"><span data-stu-id="e6a59-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="e6a59-121">[![Control deslizante y cuadro de texto están sincronizadas](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6a59-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="e6a59-122">Control deslizante y el cuadro de texto están sincronizados ([haga clic aquí para ver la imagen a tamaño completo](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e6a59-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e6a59-123">[Anterior](using-the-slider-control-with-auto-postback-cs.md)
[Siguiente](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e6a59-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
