---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Rellenar dinámicamente un Control utilizando el código de JavaScript (VB) | Documentos de Microsoft
author: wenz
description: El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="caca0-103">Rellenar dinámicamente un Control utilizando el código de JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="caca0-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="caca0-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="caca0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="caca0-105">[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="caca0-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="caca0-106">El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="caca0-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="caca0-107">También es posible desencadenar la población utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="caca0-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="caca0-108">Información general</span><span class="sxs-lookup"><span data-stu-id="caca0-108">Overview</span></span>

<span data-ttu-id="caca0-109">El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página.</span><span class="sxs-lookup"><span data-stu-id="caca0-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="caca0-110">También es posible desencadenar la población utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="caca0-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="caca0-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="caca0-111">Steps</span></span>

<span data-ttu-id="caca0-112">En primer lugar, necesita un servicio Web de ASP.NET que implementa el método que invocará el `DynamicPopulateExtender` control.</span><span class="sxs-lookup"><span data-stu-id="caca0-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="caca0-113">El servicio web implementa el método `getDate()` que espera un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web.</span><span class="sxs-lookup"><span data-stu-id="caca0-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="caca0-114">Este es el código (archivo `DynamicPopulate.vb.asmx`) que recupera la fecha actual en uno de estos tres formatos:</span><span class="sxs-lookup"><span data-stu-id="caca0-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="caca0-115">En el paso siguiente, crear un nuevo sitio ASP.NET e iniciar con el control ScriptManager de ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="caca0-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="caca0-116">A continuación, agregue un control de etiqueta (por ejemplo mediante el control HTML del mismo nombre, o la `<asp:Label />` control web) que más adelante mostrará el resultado de llamar al servicio web.</span><span class="sxs-lookup"><span data-stu-id="caca0-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="caca0-117">A continuación, se incluyen un `DynamicPopulateExtender` controlar y proporcionar información del servicio web, el control de destino, pero no el nombre del control que desencadena el rellenado se hará más adelante con JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="caca0-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="caca0-118">Ahora a la parte de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="caca0-118">Now to the JavaScript part.</span></span> <span data-ttu-id="caca0-119">El `$find()` función, definido por la biblioteca de AJAX de ASP.NET, devuelve una referencia a objetos de servidor del Kit de herramientas de Control de ASP.NET AJAX como `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="caca0-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="caca0-120">En el archivo actual, `$find("dpe")` devuelve una referencia a la `DynamicPopulateExtender` control en la página.</span><span class="sxs-lookup"><span data-stu-id="caca0-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="caca0-121">Expone un método llamado `populate()` que desencadena el proceso de rellenado dinámica.</span><span class="sxs-lookup"><span data-stu-id="caca0-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="caca0-122">El `populate()` método requiere un argumento: la clave de contexto que servirá como argumento para el `getDate()` método web.</span><span class="sxs-lookup"><span data-stu-id="caca0-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="caca0-123">Por ejemplo, `$find("dpe").populate("format1")` rellenaría la etiqueta con la fecha actual en formato de año de días del mes.</span><span class="sxs-lookup"><span data-stu-id="caca0-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="caca0-124">Para hacer que el ejemplo sea un poco más flexible, el usuario puede elegir entre varios formatos de fecha.</span><span class="sxs-lookup"><span data-stu-id="caca0-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="caca0-125">Para cada uno de ellos, se muestra un botón de radio.</span><span class="sxs-lookup"><span data-stu-id="caca0-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="caca0-126">Una vez el usuario hace clic en un botón de radio, código de JavaScript rellena dinámicamente la etiqueta con el formato de fecha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="caca0-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="caca0-127">Estos son los botones de opción:</span><span class="sxs-lookup"><span data-stu-id="caca0-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="caca0-128">Tenga en cuenta que en el contexto de un botón de opción, la expresión de JavaScript `this.value` hace referencia al valor del botón actual, que resulta ser exactamente la misma información de la `getDate()` método puede trabajar con.</span><span class="sxs-lookup"><span data-stu-id="caca0-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="caca0-129">[![Al hacer clic en el botón recupera la fecha desde el servidor, en el formato especificado](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="caca0-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="caca0-130">Al hacer clic en el botón recupera la fecha desde el servidor, en el formato especificado ([haga clic aquí para ver la imagen a tamaño completo](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="caca0-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="caca0-131">[Anterior](dynamically-populating-a-control-vb.md)
> [Siguiente](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="caca0-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
