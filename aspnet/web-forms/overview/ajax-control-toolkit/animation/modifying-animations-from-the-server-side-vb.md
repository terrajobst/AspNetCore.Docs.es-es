---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modificar las animaciones desde el lado del servidor (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden también...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ff8685b44e7d0c2261ea7b9f1ca6397ae05ce53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811171"
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="ff1c6-104">Modificar las animaciones desde el lado del servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="ff1c6-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="ff1c6-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ff1c6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ff1c6-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ff1c6-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="ff1c6-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ff1c6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff1c6-108">También se pueden cambiar las animaciones en el lado servidor</span><span class="sxs-lookup"><span data-stu-id="ff1c6-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="ff1c6-109">Información general</span><span class="sxs-lookup"><span data-stu-id="ff1c6-109">Overview</span></span>

<span data-ttu-id="ff1c6-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ff1c6-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ff1c6-111">También se pueden cambiar las animaciones en el lado servidor</span><span class="sxs-lookup"><span data-stu-id="ff1c6-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="ff1c6-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="ff1c6-112">Steps</span></span>

<span data-ttu-id="ff1c6-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="ff1c6-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="ff1c6-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="ff1c6-116">El resto del código se ejecuta en el lado del servidor y no usa marcado; en su lugar, utiliza código para crear el `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="ff1c6-117">Sin embargo, el Kit de herramientas de Control actualmente no proporciona un acceso de API para crear las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="ff1c6-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="ff1c6-118">Sin embargo, es posible establecer el `AnimationExtender`propiedad Animations en una cadena que contiene el marcado XML que se usa al asignar las animaciones mediante declaración.</span><span class="sxs-lookup"><span data-stu-id="ff1c6-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="ff1c6-119">Con el fin de crear el XML que no debe contener el `<Animations>` elemento podría usar XML de .NET Framework admite o, como se muestra en el código siguiente, basta con que proporcione la cadena:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="ff1c6-120">Por último, agregue el `AnimationExtender` en el control a la página actual, el `<form runat="server">` elemento, asegurándose de que la animación se incluye y se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="ff1c6-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="ff1c6-121">[![La animación se crea con C# / VB código de servidor](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ff1c6-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="ff1c6-122">La animación se crea con C# / VB código de servidor ([haga clic aquí para ver imagen en tamaño completo](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ff1c6-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ff1c6-123">[Anterior](triggering-an-animation-in-another-control-vb.md)
> [Siguiente](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ff1c6-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
