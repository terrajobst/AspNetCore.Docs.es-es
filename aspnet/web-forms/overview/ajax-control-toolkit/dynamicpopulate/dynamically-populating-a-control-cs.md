---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Rellenar dinámicamente un Control (C#) | Documentos de Microsoft
author: wenz
description: El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 113b8c043c14e4ebc476b021884dd1430757452a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878612"
---
<a name="dynamically-populating-a-control-c"></a><span data-ttu-id="dc847-103">Rellenar dinámicamente un Control (C#)</span><span class="sxs-lookup"><span data-stu-id="dc847-103">Dynamically Populating a Control (C#)</span></span>
====================
<span data-ttu-id="dc847-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dc847-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dc847-105">[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dc847-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="dc847-106">El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="dc847-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="dc847-107">Información general</span><span class="sxs-lookup"><span data-stu-id="dc847-107">Overview</span></span>

<span data-ttu-id="dc847-108">El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="dc847-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="dc847-109">Este tutorial muestra cómo configurar esta opción.</span><span class="sxs-lookup"><span data-stu-id="dc847-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="dc847-110">Pasos</span><span class="sxs-lookup"><span data-stu-id="dc847-110">Steps</span></span>

<span data-ttu-id="dc847-111">En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamar `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="dc847-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="dc847-112">La clase de servicio web requiere la `ScriptService` atributo que se define en `Microsoft.Web.Script.Services`; en caso contrario, AJAX de ASP.NET no se puede crear el proxy de JavaScript del lado cliente para el servicio web y a su vez es necesario para `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="dc847-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="dc847-113">El método web debe contar con que un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web.</span><span class="sxs-lookup"><span data-stu-id="dc847-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="dc847-114">El siguiente servicio web devuelve la fecha actual en un formato representado por la `contextKey` argumento:</span><span class="sxs-lookup"><span data-stu-id="dc847-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="dc847-115">El servicio web, a continuación, se guarda como `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="dc847-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="dc847-116">Como alternativa, podría implementar el `getDate()` método como un método de página dentro de la página ASP.NET real con el `DynamicPopulate` control.</span><span class="sxs-lookup"><span data-stu-id="dc847-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="dc847-117">En el paso siguiente, cree un nuevo archivo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dc847-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="dc847-118">Como siempre, el primer paso es incluir la `ScriptManager` en la página actual para cargar la biblioteca de AJAX de ASP.NET y para realizar el trabajo del Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="dc847-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="dc847-119">A continuación, agregue un control de etiqueta (por ejemplo mediante el control HTML del mismo nombre, o la &lt; `asp:Label`  / &gt; control web) que más adelante mostrará el resultado de llamar al servicio web.</span><span class="sxs-lookup"><span data-stu-id="dc847-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="dc847-120">Un botón HTML (como un control HTML, puesto que no se requiere un postback al servidor), a continuación, se usará para desencadenar la población dinámica:</span><span class="sxs-lookup"><span data-stu-id="dc847-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="dc847-121">Por último, necesitamos la `DynamicPopulateExtender` control para conexión las cosas.</span><span class="sxs-lookup"><span data-stu-id="dc847-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="dc847-122">Los siguientes atributos se establecerá (excepto los obvios, `ID` y `runat` = `"server"`):</span><span class="sxs-lookup"><span data-stu-id="dc847-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="dc847-123">`TargetControlID` dónde colocar el resultado de llamar al servicio web</span><span class="sxs-lookup"><span data-stu-id="dc847-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="dc847-124">`ServicePath` ruta de acceso al servicio web (omitir si desea utilizar un método de página)</span><span class="sxs-lookup"><span data-stu-id="dc847-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="dc847-125">`ServiceMethod` nombre del método web o método de página</span><span class="sxs-lookup"><span data-stu-id="dc847-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="dc847-126">`ContextKey` información de contexto en que se enviará al servicio web</span><span class="sxs-lookup"><span data-stu-id="dc847-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="dc847-127">`PopulateTriggerControlID` elemento que desencadena la llamada del servicio web</span><span class="sxs-lookup"><span data-stu-id="dc847-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="dc847-128">`ClearContentsDuringUpdate` Si desea vaciar el elemento de destino durante la llamada de servicio web</span><span class="sxs-lookup"><span data-stu-id="dc847-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="dc847-129">Como puede ver, el control requiere cierta información pero puesta en marcha de todo lo que es bastante sencilla.</span><span class="sxs-lookup"><span data-stu-id="dc847-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="dc847-130">Este es el marcado para el `DynamicPopulateExtender` control en el escenario actual:</span><span class="sxs-lookup"><span data-stu-id="dc847-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="dc847-131">Ejecute la página ASP.NET en el explorador y haga clic en el botón; recibirá la fecha actual en formato de año de días del mes.</span><span class="sxs-lookup"><span data-stu-id="dc847-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="dc847-132">[![Al hacer clic en el botón recupera la fecha del servidor](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dc847-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="dc847-133">Al hacer clic en el botón recupera la fecha del servidor ([haga clic aquí para ver la imagen a tamaño completo](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dc847-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dc847-134">Siguiente</span><span class="sxs-lookup"><span data-stu-id="dc847-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
