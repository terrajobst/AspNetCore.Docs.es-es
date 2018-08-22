---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipular propiedades DropShadow desde el código de cliente (C#) | Microsoft Docs
author: wenz
description: Personalizar la interfaz de edición de DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: f751e2378621a6ab74f9f15fe51fd18bdd8b4070
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835937"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="7738a-103">Manipular propiedades DropShadow desde el código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="7738a-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="7738a-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7738a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7738a-105">[Descargar código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7738a-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="7738a-106">El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="7738a-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7738a-107">También se pueden cambiar las propiedades de este extensor con el código de JavaScript de cliente.</span><span class="sxs-lookup"><span data-stu-id="7738a-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="7738a-108">Información general</span><span class="sxs-lookup"><span data-stu-id="7738a-108">Overview</span></span>

<span data-ttu-id="7738a-109">El control DropShadow en AJAX Control Toolkit amplía un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="7738a-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7738a-110">También se pueden cambiar las propiedades de este extensor con el código de JavaScript de cliente.</span><span class="sxs-lookup"><span data-stu-id="7738a-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7738a-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="7738a-111">Steps</span></span>

<span data-ttu-id="7738a-112">El código comienza con un panel que contiene algunas líneas de texto:</span><span class="sxs-lookup"><span data-stu-id="7738a-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="7738a-113">La clase CSS asociada le ofrece el panel de un color de fondo bueno:</span><span class="sxs-lookup"><span data-stu-id="7738a-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="7738a-114">El `DropShadowExtender` se agrega para ampliar el panel con un efecto de sombra paralela, establecido en 50% de opacidad:</span><span class="sxs-lookup"><span data-stu-id="7738a-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="7738a-115">A continuación, ASP.NET AJAX `ScriptManager` control habilita el Kit de herramientas de Control para que funcione:</span><span class="sxs-lookup"><span data-stu-id="7738a-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="7738a-116">Otro panel contiene dos vínculos de JavaScript para establecer la opacidad de la sombra paralela: el vínculo menos reduce la opacidad de la sombra, aumenta el vínculo del signo más.</span><span class="sxs-lookup"><span data-stu-id="7738a-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="7738a-117">La función de JavaScript `changeOpacity()` , a continuación, debe buscar el `DropShadowExtender` control en la página.</span><span class="sxs-lookup"><span data-stu-id="7738a-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="7738a-118">ASP.NET AJAX se define la `$find()` método para exactamente de esa tarea.</span><span class="sxs-lookup"><span data-stu-id="7738a-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="7738a-119">A continuación, la `get_Opacity()` método recupera la opacidad actual, el `set_Opacity()` método establece esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="7738a-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="7738a-120">El código de JavaScript, a continuación, coloca el valor de opacidad actual en el `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="7738a-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="7738a-121">[![Se cambia la opacidad del lado cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7738a-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="7738a-122">Se cambia la opacidad del lado cliente ([haga clic aquí para ver imagen en tamaño completo](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7738a-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7738a-123">[Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Siguiente](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7738a-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
