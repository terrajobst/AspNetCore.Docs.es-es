---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Controlar los Postbacks desde un ModalPopup (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Debe tener especial cuidado cuando un punto de venta...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f27d28b9851f8e26c207a6bc495e98ad70577a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838127"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="9f3f5-104">Controlar los Postbacks desde un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="9f3f5-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="9f3f5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9f3f5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9f3f5-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9f3f5-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="9f3f5-107">El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9f3f5-108">Debe tener cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="9f3f5-109">Información general</span><span class="sxs-lookup"><span data-stu-id="9f3f5-109">Overview</span></span>

<span data-ttu-id="9f3f5-110">El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="9f3f5-111">Debe tener cuidado especial cuando se crea una devolución de datos desde dentro de la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="9f3f5-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="9f3f5-112">Steps</span></span>

<span data-ttu-id="9f3f5-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="9f3f5-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="9f3f5-114">A continuación, agregue un panel que actúa como el elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="9f3f5-115">Allí, el usuario puede escribir un nombre y una dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="9f3f5-116">Se utiliza un botón para cerrar el cuadro emergente y guardar la información.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="9f3f5-117">Tenga en cuenta que el `OnClick` atributo está establecido para que se produce un postback cuando se hace clic en este botón:</span><span class="sxs-lookup"><span data-stu-id="9f3f5-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="9f3f5-118">La página en Sí consta de dos etiquetas para exactamente la misma información: dirección de correo electrónico y nombre.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="9f3f5-119">Se utiliza un botón para desencadenar el elemento emergente modal:</span><span class="sxs-lookup"><span data-stu-id="9f3f5-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="9f3f5-120">Para hacer que aparezca la ventana emergente, agregue el `ModalPopupExtender` control.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="9f3f5-121">Establecer el `PopupControlID` atributo ID del panel y `TargetControlID` para el identificador del botón:</span><span class="sxs-lookup"><span data-stu-id="9f3f5-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="9f3f5-122">Ahora cada vez que el `Save` dentro de la ventana emergente modal se hace clic en botón, el servidor `SaveData()` se ejecuta el método.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="9f3f5-123">Allí, puede guardar los datos introducidos en un almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="9f3f5-124">Por simplicidad, los nuevos datos solo se genera en la etiqueta:</span><span class="sxs-lookup"><span data-stu-id="9f3f5-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="9f3f5-125">Además, los controles de cuadro de texto dentro de la ventana emergente modal deben rellenarse con el nombre actual y el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="9f3f5-126">Sin embargo esto solo es necesario cuando se produce ninguna devolución de datos.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="9f3f5-127">Si se produce un postback, la característica de viewstate ASP.NET rellenará automáticamente con los valores apropiados en los cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="9f3f5-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="9f3f5-128">[![El elemento emergente modal provoca una devolución de datos](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9f3f5-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="9f3f5-129">El elemento emergente modal produce un postback ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9f3f5-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9f3f5-130">[Anterior](using-modalpopup-with-a-repeater-control-cs.md)
> [Siguiente](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9f3f5-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
