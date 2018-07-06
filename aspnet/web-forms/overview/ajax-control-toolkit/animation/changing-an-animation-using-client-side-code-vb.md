---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Cambiar una animación con código del lado cliente (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. La animación puede también...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: afb275e4e5155ceccb7e696cbf9d00de99a455f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840737"
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="91474-104">Cambiar una animación con código del lado cliente (VB)</span><span class="sxs-lookup"><span data-stu-id="91474-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="91474-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="91474-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="91474-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="91474-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="91474-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="91474-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="91474-108">También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="91474-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="91474-109">Información general</span><span class="sxs-lookup"><span data-stu-id="91474-109">Overview</span></span>

<span data-ttu-id="91474-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="91474-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="91474-111">También se puede cambiar la animación mediante código personalizado de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="91474-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="91474-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="91474-112">Steps</span></span>

<span data-ttu-id="91474-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="91474-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="91474-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="91474-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="91474-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="91474-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="91474-116">La animación real se inicia mediante un botón HTML:</span><span class="sxs-lookup"><span data-stu-id="91474-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="91474-117">A continuación, agregue el `AnimationExtender` a la página, que proporciona un `ID`, el `TargetControlID` atributo y el obligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="91474-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="91474-118">Tenga en cuenta que no hay ningún `<Animations>` nodo dentro de la `AnimationExtender` control.</span><span class="sxs-lookup"><span data-stu-id="91474-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="91474-119">Código JavaScript personalizado se utiliza para proporcionar las animaciones que se usará con el control.</span><span class="sxs-lookup"><span data-stu-id="91474-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="91474-120">Igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil para asignar una animación para el extensor todavía.</span><span class="sxs-lookup"><span data-stu-id="91474-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="91474-121">Sin embargo, el extensor exponer varios métodos para leer y escribir las animaciones registrado con los distintos eventos (`OnClick`, `OnLoad`, y así sucesivamente).</span><span class="sxs-lookup"><span data-stu-id="91474-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="91474-122">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="91474-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="91474-123">El formato del valor devuelto de la `get_*()` funciones y el formato del argumento para el `set_*()` functions es una cadena JSON, que proporciona una representación de objeto de cuál sería el marcado XML.</span><span class="sxs-lookup"><span data-stu-id="91474-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="91474-124">Actualmente, no hay ninguna manera de pasar un objeto, pero es posible leer un objeto de una animación (`get_OnXXXBehavior()` métodos).</span><span class="sxs-lookup"><span data-stu-id="91474-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="91474-125">Esta es una cadena JSON (sin las comillas delimitadoras y presentarla convenientemente) que representa una animación que se desencadene el botón, pero el panel de la animación, cambiar su tamaño y atenúa al mismo tiempo:</span><span class="sxs-lookup"><span data-stu-id="91474-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="91474-126">El siguiente código JavaScript asigna este descripting JSON para el `OnClick` animación del extensor actual y lo ejecuta:</span><span class="sxs-lookup"><span data-stu-id="91474-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="91474-127">[![La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="91474-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="91474-128">La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco de marcado) ([haga clic aquí para ver imagen en tamaño completo](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="91474-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="91474-129">[Anterior](executing-animations-using-client-side-code-vb.md)
> [Siguiente](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="91474-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
