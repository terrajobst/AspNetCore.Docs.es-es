---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Controlar los Postbacks desde un Control emergente sin un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Cuando se produce un postback en unidades de búsqueda...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dcb2b09279ec6400465f79fadc2a1b6c72f8f07
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829737"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="5edb5-104">Controlar los Postbacks desde un Control emergente sin un UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="5edb5-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="5edb5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5edb5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5edb5-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5edb5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="5edb5-107">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="5edb5-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5edb5-108">Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.</span><span class="sxs-lookup"><span data-stu-id="5edb5-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="5edb5-109">Información general</span><span class="sxs-lookup"><span data-stu-id="5edb5-109">Overview</span></span>

<span data-ttu-id="5edb5-110">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="5edb5-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5edb5-111">Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.</span><span class="sxs-lookup"><span data-stu-id="5edb5-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="5edb5-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="5edb5-112">Steps</span></span>

<span data-ttu-id="5edb5-113">Cuando se usa un `PopupControl` con una devolución de datos, pero sin tener un `UpdatePanel` en la página, el Kit de herramientas de Control no ofrece una manera de determinar qué elemento de cliente ha desencadenado la ventana emergente que a su vez la originaron.</span><span class="sxs-lookup"><span data-stu-id="5edb5-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="5edb5-114">Sin embargo, un pequeño truco proporciona una solución alternativa para este escenario.</span><span class="sxs-lookup"><span data-stu-id="5edb5-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="5edb5-115">En primer lugar, aquí es la configuración básica: dos cuadros de texto que desencadenen la mismo ventana emergente, un calendario.</span><span class="sxs-lookup"><span data-stu-id="5edb5-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="5edb5-116">Dos `PopupControlExtenders` reunir los cuadros de texto y el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="5edb5-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="5edb5-117">La idea básica consiste en Agregar un campo de formulario oculto en el &lt; `form` &gt; elemento que contiene el cuadro de texto que se inicia la ventana emergente:</span><span class="sxs-lookup"><span data-stu-id="5edb5-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="5edb5-118">Cuando se carga la página, el código de JavaScript agrega un controlador de eventos en ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, se escribe su nombre en el campo de formulario oculto:</span><span class="sxs-lookup"><span data-stu-id="5edb5-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="5edb5-119">En el código del lado servidor, se debe leer el valor del campo oculto.</span><span class="sxs-lookup"><span data-stu-id="5edb5-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="5edb5-120">Puesto que son triviales para manipular campos ocultos de formulario, se requiere un enfoque de lista de permitidos para validar el valor hidden.</span><span class="sxs-lookup"><span data-stu-id="5edb5-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="5edb5-121">Una vez identificado el cuadro de texto correcto, la fecha del calendario se escribe en él.</span><span class="sxs-lookup"><span data-stu-id="5edb5-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="5edb5-122">[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5edb5-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="5edb5-123">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5edb5-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="5edb5-124">[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5edb5-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="5edb5-125">Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5edb5-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5edb5-126">Anterior</span><span class="sxs-lookup"><span data-stu-id="5edb5-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
