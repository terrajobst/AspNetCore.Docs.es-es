---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: "Manipular propiedades de sombra paralela desde el código de cliente (VB) | Documentos de Microsoft"
author: wenz
description: "El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela. Propiedades de este extensor también pueden cambiarse mediante cliente JavaScrip..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 706ccb5a95873aad4c1b9e0942ab06cf4162710a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="22056-104">Manipular propiedades de sombra paralela desde el código de cliente (VB)</span><span class="sxs-lookup"><span data-stu-id="22056-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="22056-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="22056-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="22056-106">[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="22056-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="22056-107">El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="22056-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="22056-108">Propiedades de este extensor también se pueden cambiar con el código de JavaScript de cliente.</span><span class="sxs-lookup"><span data-stu-id="22056-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="22056-109">Información general</span><span class="sxs-lookup"><span data-stu-id="22056-109">Overview</span></span>

<span data-ttu-id="22056-110">El control de sombra paralela en el Kit de herramientas de Control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="22056-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="22056-111">Propiedades de este extensor también se pueden cambiar con el código de JavaScript de cliente.</span><span class="sxs-lookup"><span data-stu-id="22056-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="22056-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="22056-112">Steps</span></span>

<span data-ttu-id="22056-113">El código comienza con un panel que contiene algunas líneas de texto:</span><span class="sxs-lookup"><span data-stu-id="22056-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="22056-114">La clase CSS asociada proporciona el panel de un color de fondo "nice":</span><span class="sxs-lookup"><span data-stu-id="22056-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="22056-115">El `DropShadowExtender` se agrega para ampliar el panel con un conjunto al 50% de opacidad del efecto de sombra paralela:</span><span class="sxs-lookup"><span data-stu-id="22056-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="22056-116">A continuación, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:</span><span class="sxs-lookup"><span data-stu-id="22056-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="22056-117">Otro panel contiene dos vínculos de JavaScript para establecer la opacidad de la sombra paralela: el vínculo menos reduce la opacidad de la sombra, el vínculo más aumenta.</span><span class="sxs-lookup"><span data-stu-id="22056-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="22056-118">La función de JavaScript `changeOpacity()` , a continuación, debe buscar primero la `DropShadowExtender` control en la página.</span><span class="sxs-lookup"><span data-stu-id="22056-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="22056-119">AJAX de ASP.NET define la `$find()` método para exactamente esa tarea.</span><span class="sxs-lookup"><span data-stu-id="22056-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="22056-120">A continuación, la `get_Opacity()` método recupera la opacidad actual, el `set_Opacity()` método establece esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="22056-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="22056-121">El código de JavaScript, a continuación, coloca el valor de opacidad actual en el `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="22056-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="22056-122">[![Se cambia la opacidad del lado cliente](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22056-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="22056-123">Se cambia la opacidad del lado cliente ([haga clic aquí para ver la imagen a tamaño completo](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="22056-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="22056-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="22056-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
