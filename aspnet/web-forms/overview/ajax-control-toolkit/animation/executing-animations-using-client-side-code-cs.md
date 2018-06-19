---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Ejecutar animaciones utilizando el código de cliente (C#) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. La ejecución de la animación...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870744"
---
<a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="418a4-104">Ejecutar animaciones utilizando el código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="418a4-104">Executing Animations Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="418a4-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="418a4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="418a4-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="418a4-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="418a4-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="418a4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="418a4-108">También se puede desencadenar la ejecución de animación utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="418a4-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="418a4-109">Información general</span><span class="sxs-lookup"><span data-stu-id="418a4-109">Overview</span></span>

<span data-ttu-id="418a4-110">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="418a4-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="418a4-111">También se puede desencadenar la ejecución de animación utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="418a4-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="418a4-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="418a4-112">Steps</span></span>

<span data-ttu-id="418a4-113">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="418a4-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="418a4-114">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="418a4-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="418a4-115">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="418a4-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="418a4-116">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="418a4-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="418a4-117">En el `<Animations>` nodo, use `<OnClick>` ejecutar las animaciones una vez el usuario hace clic en el panel.</span><span class="sxs-lookup"><span data-stu-id="418a4-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="418a4-118">Agregue dos animaciones que se ejecute parallelly:</span><span class="sxs-lookup"><span data-stu-id="418a4-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="418a4-119">En aras de la demostración, esta animación (y cualquier otra animación creados mediante el Kit de herramientas de Control) se ejecutan utilizando el código de JavaScript, una vez que se ejecuta la página.</span><span class="sxs-lookup"><span data-stu-id="418a4-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="418a4-120">En primer lugar se necesitan tener acceso a la `AnimationExtender` control.</span><span class="sxs-lookup"><span data-stu-id="418a4-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="418a4-121">La biblioteca de AJAX de ASP.NET proporciona la `$find()` función para esta tarea:</span><span class="sxs-lookup"><span data-stu-id="418a4-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="418a4-122">El `AnimationExtender` control expone una API enriquecida, incluidos los métodos con nombres idénticos a los controladores de eventos que se utiliza en el marcado XML: `OnClick()`, `OnLoad()`, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="418a4-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="418a4-123">Por ejemplo, una llamada de la `OnClick()` el método ejecuta la animación en la `<OnClick>` elemento de la `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="418a4-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="418a4-124">Este es el código de JavaScript completo de cliente que emula el haga clic en el panel de una vez que la página se ha cargado completamente tenga en cuenta que el `pageLoad()` es usar el nombre de función que se llama mediante AJAX de ASP.NET una vez la página e incluyen han sido bibliotecas de JavaScript cargar.</span><span class="sxs-lookup"><span data-stu-id="418a4-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


<span data-ttu-id="418a4-125">[![La animación se ejecuta inmediatamente, sin un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="418a4-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="418a4-126">La animación se ejecuta inmediatamente, sin un clic del mouse ([haga clic aquí para ver la imagen a tamaño completo](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="418a4-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="418a4-127">[Anterior](modifying-animations-from-the-server-side-cs.md)
> [Siguiente](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="418a4-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
