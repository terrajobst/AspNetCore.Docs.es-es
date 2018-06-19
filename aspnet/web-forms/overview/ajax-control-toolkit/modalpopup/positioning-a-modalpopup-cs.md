---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Colocar un ModalPopup (C#) | Documentos de Microsoft
author: wenz
description: El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo el control no proporciona un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874153"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="24955-104">Colocar un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="24955-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="24955-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="24955-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="24955-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="24955-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="24955-107">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="24955-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="24955-108">Sin embargo el control proporcionan una funcionalidad integrada para colocar la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="24955-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="24955-109">Información general</span><span class="sxs-lookup"><span data-stu-id="24955-109">Overview</span></span>

<span data-ttu-id="24955-110">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="24955-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="24955-111">Sin embargo el control proporcionan una funcionalidad integrada para colocar la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="24955-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="24955-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="24955-112">Steps</span></span>

<span data-ttu-id="24955-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="24955-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="24955-114">control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="24955-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="24955-115">A continuación, agregue un panel que actúa como el elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="24955-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="24955-116">Se utiliza un botón para cerrar la ventana emergente:</span><span class="sxs-lookup"><span data-stu-id="24955-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="24955-117">Cada vez que se muestra la ventana emergente, se deben colocarse en un lugar determinado en la página.</span><span class="sxs-lookup"><span data-stu-id="24955-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="24955-118">Para esta tarea, se crea una función de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="24955-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="24955-119">Primero intenta tener acceso al panel.</span><span class="sxs-lookup"><span data-stu-id="24955-119">It first tries to access the panel.</span></span> <span data-ttu-id="24955-120">Si se realiza correctamente, la posición del panel se establece con CSS y JavaScript (cambiar la posición de la ventana emergente en le).</span><span class="sxs-lookup"><span data-stu-id="24955-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="24955-121">Sin embargo, el `ModalPopupExtender` control también intenta colocar la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="24955-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="24955-122">Por lo tanto, el código de JavaScript coloca varias veces en la ventana emergente, cada décima parte de un segundo.</span><span class="sxs-lookup"><span data-stu-id="24955-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="24955-123">Como puede ver, el valor devuelto de la `setTimeout()` método JavaScript se guarda en una variable global.</span><span class="sxs-lookup"><span data-stu-id="24955-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="24955-124">Esto permite detener la posición repetidos de la ventana emergente a petición, con el `clearTimeout()` método:</span><span class="sxs-lookup"><span data-stu-id="24955-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="24955-125">Ahora todo lo que queda por hacer es hacer que el explorador llamar a estas funciones siempre que sea adecuado.</span><span class="sxs-lookup"><span data-stu-id="24955-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="24955-126">El `movePanel()` debe llamar la función de JavaScript cuando se presiona el botón que desencadena el panel:</span><span class="sxs-lookup"><span data-stu-id="24955-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="24955-127">Y el `stopMoving()` función entra en juego cuando la ventana emergente se cierra se pueden activar en el `ModalPopupExtender` control:</span><span class="sxs-lookup"><span data-stu-id="24955-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="24955-128">[![La ventana emergente modal aparece en la posición designada](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="24955-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="24955-129">La ventana emergente modal aparece en la posición designada ([haga clic aquí para ver la imagen a tamaño completo](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="24955-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24955-130">[Anterior](handling-postbacks-from-a-modalpopup-cs.md)
> [Siguiente](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="24955-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
