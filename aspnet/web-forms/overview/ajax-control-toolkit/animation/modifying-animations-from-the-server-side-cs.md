---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modificar las animaciones desde el lado del servidor (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Las animaciones pueden también...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 83ce54b1cd2c226db36be75f61321a0fb710e0ca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376667"
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="c16c1-104">Modificar las animaciones desde el lado del servidor (C#)</span><span class="sxs-lookup"><span data-stu-id="c16c1-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="c16c1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c16c1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c16c1-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c16c1-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="c16c1-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="c16c1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c16c1-108">También se pueden cambiar las animaciones en el lado servidor</span><span class="sxs-lookup"><span data-stu-id="c16c1-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="c16c1-109">Información general</span><span class="sxs-lookup"><span data-stu-id="c16c1-109">Overview</span></span>

<span data-ttu-id="c16c1-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="c16c1-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c16c1-111">También se pueden cambiar las animaciones en el lado servidor</span><span class="sxs-lookup"><span data-stu-id="c16c1-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="c16c1-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="c16c1-112">Steps</span></span>

<span data-ttu-id="c16c1-113">En primer lugar, incluya el `ScriptManager` en la página; a continuación, ASP.NET AJAX se carga la biblioteca, lo que permite usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="c16c1-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="c16c1-114">La animación se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="c16c1-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="c16c1-115">En la clase CSS asociada para el panel, definir un color de fondo agradable y también establecer un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="c16c1-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="c16c1-116">El resto del código se ejecuta en el lado del servidor y no usa marcado; en su lugar, utiliza código para crear el `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="c16c1-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="c16c1-117">Sin embargo, el Kit de herramientas de Control actualmente no proporciona un acceso de API para crear las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="c16c1-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="c16c1-118">Sin embargo, es posible establecer el `AnimationExtender`propiedad Animations en una cadena que contiene el marcado XML que se usa al asignar las animaciones mediante declaración.</span><span class="sxs-lookup"><span data-stu-id="c16c1-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="c16c1-119">Con el fin de crear el XML que no debe contener el `<Animations>` elemento podría usar XML de .NET Framework admite o, como se muestra en el código siguiente, basta con que proporcione la cadena:</span><span class="sxs-lookup"><span data-stu-id="c16c1-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="c16c1-120">Por último, agregue el `AnimationExtender` en el control a la página actual, el `<form runat="server">` elemento, asegurándose de que la animación se incluye y se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="c16c1-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="c16c1-121">[![La animación se crea con C# / VB código de servidor](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c16c1-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="c16c1-122">La animación se crea con C# / VB código de servidor ([haga clic aquí para ver imagen en tamaño completo](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c16c1-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c16c1-123">[Anterior](triggering-an-animation-in-another-control-cs.md)
> [Siguiente](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c16c1-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
