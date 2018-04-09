---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Contraer y expandir un Panel desde JavaScript (C#) | Documentos de Microsoft
author: wenz
description: El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad de expandir o contraer su contenido un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 7baa3be7144946bde7d11afd9b1cb5f14ad9dede
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="8240e-103">Contraer y expandir un Panel desde JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="8240e-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="8240e-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8240e-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8240e-105">[Descargar código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8240e-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="8240e-106">El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo.</span><span class="sxs-lookup"><span data-stu-id="8240e-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="8240e-107">Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="8240e-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="8240e-108">Información general</span><span class="sxs-lookup"><span data-stu-id="8240e-108">Overview</span></span>

<span data-ttu-id="8240e-109">El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo.</span><span class="sxs-lookup"><span data-stu-id="8240e-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="8240e-110">Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="8240e-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="8240e-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="8240e-111">Steps</span></span>

<span data-ttu-id="8240e-112">En primer lugar, cree una nueva página ASP.NET e incluya la `ScriptManager` dentro del `<form>` elemento.</span><span class="sxs-lookup"><span data-stu-id="8240e-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="8240e-113">Esto carga la biblioteca de AJAX de ASP.NET y es necesario para el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="8240e-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="8240e-114">A continuación, cree un panel con algún texto, por lo que puede verse el efecto de expandir y contraer:</span><span class="sxs-lookup"><span data-stu-id="8240e-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="8240e-115">Como puede ver, el panel hace referencia a una clase CSS que se muestra aquí (y básicamente define un color de fondo y el ancho del panel):</span><span class="sxs-lookup"><span data-stu-id="8240e-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="8240e-116">El `CollapsiblePanelExtender` control requiere el `TargetControlID` atributo para que el Kit de herramientas sepa cuál de los paneles para contraer o expandir tras la solicitud:</span><span class="sxs-lookup"><span data-stu-id="8240e-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="8240e-117">Desafortunadamente, el dispositivo extender actualmente no expone una API específica para contraer o expandir el panel, pero algunos métodos sin documentar llevará a cabo.</span><span class="sxs-lookup"><span data-stu-id="8240e-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="8240e-118">En primer lugar, agregue tres botones HTML a la página que, a continuación, desencadenará el JavaScript del lado cliente para contraer o expandir el contenido del panel:</span><span class="sxs-lookup"><span data-stu-id="8240e-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="8240e-119">En el código de JavaScript del lado cliente (partió `<script type="text/javascript">`), el `$find()` método debe usarse para tener acceso a la `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="8240e-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="8240e-120">`$find("cpe")` Devuelve una referencia a él.</span><span class="sxs-lookup"><span data-stu-id="8240e-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="8240e-121">Desde aquí, los métodos específicos resolverá la tarea en cuestión.</span><span class="sxs-lookup"><span data-stu-id="8240e-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="8240e-122">El método de apertura (expandir) se denomina el panel `_doOpen()`; el código siguiente implementa la `doOpen()` función se llama cuando se hace clic en el primer botón:</span><span class="sxs-lookup"><span data-stu-id="8240e-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="8240e-123">Para cerrar o contraer el panel, el `_doClose()` método debe ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="8240e-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="8240e-124">Por lo que cuando el usuario hace clic en el segundo botón, se llama al código de JavaScript siguiente:</span><span class="sxs-lookup"><span data-stu-id="8240e-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="8240e-125">El tercer botón alterna el estado del panel: de contraer para expandir y viceversa.</span><span class="sxs-lookup"><span data-stu-id="8240e-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="8240e-126">El `CollapsiblePanelExtender` expone el `toggle()` método que hace exactamente eso: invierte el estado del panel.</span><span class="sxs-lookup"><span data-stu-id="8240e-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="8240e-127">Sin embargo, también es otro método (que se usa internamente en el `toggle()` (método)): el `get_Collapsed()` método de la `CollapsiblePanelExtender()` nos indica si el panel está contraído o no.</span><span class="sxs-lookup"><span data-stu-id="8240e-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="8240e-128">Dependiendo del valor devuelto de esta función, el panel es, a continuación, ya sea expandido (`_doOpen()` método) o contraer (`_doClose()`) método:</span><span class="sxs-lookup"><span data-stu-id="8240e-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="8240e-129">[![El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8240e-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="8240e-130">El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa ([haga clic aquí para ver la imagen a tamaño completo](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8240e-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8240e-131">Siguiente</span><span class="sxs-lookup"><span data-stu-id="8240e-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
