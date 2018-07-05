---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usar DynamicPopulate con un Control de usuario y JavaScript (C#) | Microsoft Docs
author: wenz
description: El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino de t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cba1647e69cbaebf0accb745278590e357d8c2c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370424"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="6f813-103">Usar DynamicPopulate con un Control de usuario y JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="6f813-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="6f813-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6f813-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6f813-105">[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6f813-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="6f813-106">El control DynamicPopulate de ASP.NET AJAX Control Toolkit llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página.</span><span class="sxs-lookup"><span data-stu-id="6f813-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="6f813-107">También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="6f813-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="6f813-108">Sin embargo un cuidado especial tiene que ser tomada cuando el dispositivo extender reside en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="6f813-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="6f813-109">Información general</span><span class="sxs-lookup"><span data-stu-id="6f813-109">Overview</span></span>

<span data-ttu-id="6f813-110">El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de página) y rellena el valor resultante en un control de destino en la página, sin una actualización de la página.</span><span class="sxs-lookup"><span data-stu-id="6f813-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="6f813-111">También es posible desencadenar la población mediante código personalizado de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="6f813-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="6f813-112">Sin embargo un cuidado especial tiene que ser tomada cuando el dispositivo extender reside en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="6f813-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="6f813-113">Pasos</span><span class="sxs-lookup"><span data-stu-id="6f813-113">Steps</span></span>

<span data-ttu-id="6f813-114">En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamará el `DynamicPopulateExtender` control.</span><span class="sxs-lookup"><span data-stu-id="6f813-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="6f813-115">El servicio web implementa el método `getDate()` que espera un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web.</span><span class="sxs-lookup"><span data-stu-id="6f813-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="6f813-116">Este es el código (archivo `DynamicPopulate.cs.asmx`) que recupera la fecha actual en uno de los tres formatos:</span><span class="sxs-lookup"><span data-stu-id="6f813-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="6f813-117">En el paso siguiente, creará un nuevo control de usuario (`.ascx` archivo), conocido por la declaración siguiente en la primera línea:</span><span class="sxs-lookup"><span data-stu-id="6f813-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="6f813-118">Un &lt; `label` &gt; elemento que se usará para mostrar los datos procedentes del servidor.</span><span class="sxs-lookup"><span data-stu-id="6f813-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="6f813-119">También en el archivo de control de usuario, usaremos tres botones de radio, cada uno que representa uno de los tres formatos de fecha admitidos por el servicio web.</span><span class="sxs-lookup"><span data-stu-id="6f813-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="6f813-120">Cuando el usuario hace clic en uno de los botones de radio, el explorador ejecutará el código de JavaScript que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="6f813-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="6f813-121">Este código tiene acceso a la `DynamicPopulateExtender` (no se preocupe el identificador extraño todavía, esto se explicará más adelante) y desencadena el rellenado con datos dinámico.</span><span class="sxs-lookup"><span data-stu-id="6f813-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="6f813-122">En el contexto del botón de radio actual, `this.value` hace referencia a su valor que es `format1`, `format2` o `format3` exactamente lo que espera el método web.</span><span class="sxs-lookup"><span data-stu-id="6f813-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="6f813-123">Lo único que no se encuentra en el control de usuario todavía es el `DynamicPopulateExtender` control lo que vincula los botones de radio con el servicio web.</span><span class="sxs-lookup"><span data-stu-id="6f813-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="6f813-124">Nuevo es posible que tenga en cuenta el identificador extraño utilizado en el control: `mcd1$myDate` en lugar de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="6f813-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="6f813-125">Anteriormente, el código de JavaScript usado `mcd1_dpe1` para tener acceso a la `DynamicPopulateExtender` en lugar de `dpe1`. Esta estrategia de nomenclatura es un requisito especial cuando se usa `DynamicPopulateExtender` dentro de un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="6f813-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="6f813-126">Además, tiene que insertar el control de usuario de forma específica para que todo funcione.</span><span class="sxs-lookup"><span data-stu-id="6f813-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="6f813-127">Crear una nueva página ASP.NET y registrar un prefijo de etiqueta del control de usuario que acaba de implementar:</span><span class="sxs-lookup"><span data-stu-id="6f813-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="6f813-128">A continuación, se incluyen ASP.NET AJAX `ScriptManager` control en la página nueva:</span><span class="sxs-lookup"><span data-stu-id="6f813-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="6f813-129">Por último, agregue el control de usuario a la página.</span><span class="sxs-lookup"><span data-stu-id="6f813-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="6f813-130">Solo tiene que establecer su `ID` atributo (y `runat="server"`, por supuesto), pero también debe establecer en un nombre específico: `mcd1` puesto que es el prefijo usado en el control de usuario para acceder a él mediante JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6f813-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="6f813-131">Y listo.</span><span class="sxs-lookup"><span data-stu-id="6f813-131">And that's it!</span></span> <span data-ttu-id="6f813-132">La página se comporta según lo esperado: un usuario hace clic en uno de los botones de radio, el control en el Kit de herramientas, llama al servicio web y muestra la fecha actual en el formato deseado.</span><span class="sxs-lookup"><span data-stu-id="6f813-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="6f813-133">[![Los botones de radio residen en un control de usuario](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6f813-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="6f813-134">Los botones de radio residen en un control de usuario ([haga clic aquí para ver imagen en tamaño completo](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6f813-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6f813-135">[Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Siguiente](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6f813-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
