---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Cambiar una animación con código del lado cliente (C#) | Documentos de Microsoft
author: wenz
description: El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. También puede la animación...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f572efeb012d88ab15740bab7b0e4383572f3f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="a7e35-104">Cambiar una animación con código del lado cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="a7e35-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="a7e35-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a7e35-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a7e35-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a7e35-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="a7e35-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a7e35-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a7e35-108">La animación también se puede cambiar utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="a7e35-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a7e35-109">Información general</span><span class="sxs-lookup"><span data-stu-id="a7e35-109">Overview</span></span>

<span data-ttu-id="a7e35-110">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a7e35-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a7e35-111">La animación también se puede cambiar utilizando el código de JavaScript de cliente personalizada.</span><span class="sxs-lookup"><span data-stu-id="a7e35-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a7e35-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="a7e35-112">Steps</span></span>

<span data-ttu-id="a7e35-113">En primer lugar, se incluyen el `ScriptManager` en la página; a continuación, AJAX de ASP.NET se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="a7e35-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a7e35-114">La animación se aplicará a un panel de texto que el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a7e35-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="a7e35-115">En la clase CSS asociada para el panel, definir un color de fondo "nice" y también establece un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="a7e35-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="a7e35-116">Un botón HTML inicia la animación real:</span><span class="sxs-lookup"><span data-stu-id="a7e35-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a7e35-117">A continuación, agregue el `AnimationExtender` a la página, proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a7e35-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="a7e35-118">Tenga en cuenta que no hay ningún `<Animations>` nodo dentro de la `AnimationExtender` control.</span><span class="sxs-lookup"><span data-stu-id="a7e35-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="a7e35-119">Código de JavaScript personalizado se utiliza para proporcionar las animaciones que se utilizará con el control.</span><span class="sxs-lookup"><span data-stu-id="a7e35-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="a7e35-120">Al igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil de asignar una animación para el dispositivo extender aún.</span><span class="sxs-lookup"><span data-stu-id="a7e35-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="a7e35-121">Sin embargo, el dispositivo extender exponer varios métodos para leer y escribir las animaciones registrado con los distintos eventos (`OnClick`, `OnLoad`, y así sucesivamente).</span><span class="sxs-lookup"><span data-stu-id="a7e35-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="a7e35-122">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="a7e35-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="a7e35-123">El formato del valor devuelto de la `get_*()` funciones y el formato del argumento para el `set_*()` funciones es una cadena JSON, que proporciona una representación de objeto de lo que sería el marcado XML.</span><span class="sxs-lookup"><span data-stu-id="a7e35-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="a7e35-124">Actualmente, no hay ninguna manera de pasar un objeto, pero es posible leer un objeto de una animación (`get_OnXXXBehavior()` métodos).</span><span class="sxs-lookup"><span data-stu-id="a7e35-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="a7e35-125">Esta es una cadena JSON (sin las comillas delimitadoras y un formato bien) que representa una animación desencadenada por el botón, pero el panel de la animación, cambiando su tamaño y desaparecer gradualmente al mismo tiempo:</span><span class="sxs-lookup"><span data-stu-id="a7e35-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="a7e35-126">El siguiente código de JavaScript asigna este descripting JSON para el `OnClick` animación del extensor actual y la ejecuta:</span><span class="sxs-lookup"><span data-stu-id="a7e35-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="a7e35-127">[![La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7e35-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a7e35-128">La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado) ([haga clic aquí para ver la imagen a tamaño completo](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a7e35-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7e35-129">[Anterior](executing-animations-using-client-side-code-cs.md)
> [Siguiente](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a7e35-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
