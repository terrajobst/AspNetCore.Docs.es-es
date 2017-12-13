---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Controla las devoluciones de datos desde un ModalPopup (VB) | Documentos de Microsoft
author: wenz
description: El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener especial cuidado cuando un punto de venta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 7915d555fef363f41aa51bd77f0183c97c2f3769
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="22606-104">Controla las devoluciones de datos desde un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="22606-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="22606-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="22606-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="22606-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="22606-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="22606-107">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="22606-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="22606-108">Debe tener un cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="22606-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="22606-109">Información general</span><span class="sxs-lookup"><span data-stu-id="22606-109">Overview</span></span>

<span data-ttu-id="22606-110">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="22606-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="22606-111">Debe tener un cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="22606-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="22606-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="22606-112">Steps</span></span>

<span data-ttu-id="22606-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="22606-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="22606-114">A continuación, agregue un panel que actúa como el elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="22606-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="22606-115">No existe, el usuario puede escribir un nombre y una dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="22606-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="22606-116">Se utiliza un botón para cerrar la ventana emergente y guardar la información.</span><span class="sxs-lookup"><span data-stu-id="22606-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="22606-117">Tenga en cuenta que el `OnClick` atributo está establecido para que se produce un postback cuando se hace clic en este botón:</span><span class="sxs-lookup"><span data-stu-id="22606-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="22606-118">La propia página consta de dos etiquetas para exactamente la misma información: nombre y direcciones de correo.</span><span class="sxs-lookup"><span data-stu-id="22606-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="22606-119">Un botón se usa para desencadenar la ventana emergente modal:</span><span class="sxs-lookup"><span data-stu-id="22606-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="22606-120">Para hacer que aparezca la ventana emergente, agregue el `ModalPopupExtender` control.</span><span class="sxs-lookup"><span data-stu-id="22606-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="22606-121">Establecer el `PopupControlID` atributo ID del panel y `TargetControlID` para el identificador del botón:</span><span class="sxs-lookup"><span data-stu-id="22606-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="22606-122">Ahora cada vez que la `Save` dentro de la ventana emergente modal se hace clic en botón, el lado del servidor `SaveData()` se ejecuta el método.</span><span class="sxs-lookup"><span data-stu-id="22606-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="22606-123">Allí, puede guardar los datos introducidos en un almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="22606-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="22606-124">Por simplicidad, los nuevos datos solo se genera en la etiqueta:</span><span class="sxs-lookup"><span data-stu-id="22606-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="22606-125">Además, los controles de cuadro de texto dentro de la ventana emergente modal deben rellenarse con el nombre actual y el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="22606-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="22606-126">Pero esto solo es necesario cuando se produce ninguna devolución de datos.</span><span class="sxs-lookup"><span data-stu-id="22606-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="22606-127">Si hay una devolución de datos, la característica de estado de vista ASP.NET rellenará automáticamente con los valores apropiados en los cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="22606-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="22606-128">[![La ventana emergente modal provoca una devolución de datos](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22606-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="22606-129">La ventana emergente modal provoca una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="22606-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="22606-130">[Anterior](using-modalpopup-with-a-repeater-control-vb.md)
[Siguiente](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="22606-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
