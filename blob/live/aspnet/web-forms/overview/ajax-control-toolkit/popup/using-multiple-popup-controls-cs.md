---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "Uso de varios controles de menú emergente (C#) | Documentos de Microsoft"
author: wenz
description: "El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. También es posible utilizar m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="89756-104">Uso de varios controles de menú emergente (C#)</span><span class="sxs-lookup"><span data-stu-id="89756-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="89756-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="89756-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="89756-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="89756-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="89756-107">El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="89756-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="89756-108">También es posible usar más de un control de elemento emergente en una sola página.</span><span class="sxs-lookup"><span data-stu-id="89756-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="89756-109">Información general</span><span class="sxs-lookup"><span data-stu-id="89756-109">Overview</span></span>

<span data-ttu-id="89756-110">El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="89756-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="89756-111">También es posible usar más de un control de elemento emergente en una sola página.</span><span class="sxs-lookup"><span data-stu-id="89756-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="89756-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="89756-112">Steps</span></span>

<span data-ttu-id="89756-113">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="89756-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="89756-114">A continuación, agregue un panel que actúa como la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="89756-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="89756-115">En el escenario actual, el panel contiene un `Calendar` control.</span><span class="sxs-lookup"><span data-stu-id="89756-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="89756-116">Para evitar las actualizaciones de página causadas por las devoluciones de datos del calendario, el panel se coloca dentro de un `UpdatePanel` control:</span><span class="sxs-lookup"><span data-stu-id="89756-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="89756-117">La página también contiene dos cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="89756-117">The page also contains two text boxes.</span></span> <span data-ttu-id="89756-118">Para cada cuadro de texto, el calendario emergente debe aparecer una vez que se activa el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="89756-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="89756-119">Ahora ampliar cada uno de los dos cuadros de texto con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="89756-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="89756-120">El `TargetControlID` atributo proporciona el identificador del control asociado al dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="89756-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="89756-121">El `PopupControlID` atributo contiene el identificador del panel emergente.</span><span class="sxs-lookup"><span data-stu-id="89756-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="89756-122">En este caso, ambos extensores mostrar el mismo panel, pero también son posibles, paneles diferentes.</span><span class="sxs-lookup"><span data-stu-id="89756-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="89756-123">Al hacer clic en un campo de texto, un calendario aparece debajo del campo, que le permite seleccionar una fecha.</span><span class="sxs-lookup"><span data-stu-id="89756-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="89756-124">(Recibir la fecha seleccionada en los cuadros de texto se tratará en otro tutorial.)</span><span class="sxs-lookup"><span data-stu-id="89756-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="89756-125">[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="89756-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="89756-126">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="89756-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="89756-127">Siguiente</span><span class="sxs-lookup"><span data-stu-id="89756-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
