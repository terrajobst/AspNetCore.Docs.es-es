---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Controla las devoluciones de datos de un Control de elemento emergente con un UpdatePanel (VB) | Documentos de Microsoft
author: wenz
description: El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control. Debe tenerse especial cuidado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="6c09b-104">Controla las devoluciones de datos de un Control de elemento emergente con un UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="6c09b-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="6c09b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6c09b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6c09b-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6c09b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="6c09b-107">El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="6c09b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6c09b-108">Un cuidado especial tiene que tener cuidado cuando se produce un postback dentro de este tipo un menú emergente.</span><span class="sxs-lookup"><span data-stu-id="6c09b-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="6c09b-109">Información general</span><span class="sxs-lookup"><span data-stu-id="6c09b-109">Overview</span></span>

<span data-ttu-id="6c09b-110">El extensor de PopupControl en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de desencadenar un menú emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="6c09b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6c09b-111">Un cuidado especial tiene que tener cuidado cuando se produce un postback dentro de este tipo un menú emergente.</span><span class="sxs-lookup"><span data-stu-id="6c09b-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="6c09b-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="6c09b-112">Steps</span></span>

<span data-ttu-id="6c09b-113">Cuando se usa un `PopupControl` con una devolución de datos, un `UpdatePanel` puede evitar la actualización de página causada por la devolución de datos.</span><span class="sxs-lookup"><span data-stu-id="6c09b-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="6c09b-114">El marcado siguiente define un par de elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="6c09b-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="6c09b-115">Un `ScriptManager` controlar la manera en que funciona el Kit de herramientas de Control de AJAX de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6c09b-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="6c09b-116">Dos `TextBox` controla qué ambos, se desencadenará un menú emergente</span><span class="sxs-lookup"><span data-stu-id="6c09b-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="6c09b-117">Un `Panel` control que actúa como el elemento emergente</span><span class="sxs-lookup"><span data-stu-id="6c09b-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="6c09b-118">En el panel, un `Calendar` control está incrustado en un `UpdatePanel` control</span><span class="sxs-lookup"><span data-stu-id="6c09b-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="6c09b-119">Dos `PopupControlExtender` controles que asignar el panel a los cuadros de texto</span><span class="sxs-lookup"><span data-stu-id="6c09b-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="6c09b-120">Tenga en cuenta que la `OnSelectionChanged` atributo de la `Calendar` se establece el control.</span><span class="sxs-lookup"><span data-stu-id="6c09b-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="6c09b-121">Por lo que cuando el usuario selecciona una fecha en el calendario, se produce una devolución de datos y el método de servidor `c1_SelectionChanged()` se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="6c09b-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="6c09b-122">Dentro de ese método, se debe recuperar la fecha actual y vuelve a escribir en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="6c09b-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="6c09b-123">La sintaxis para el que es como sigue: en primer lugar, un proxy del objeto para el `PopupControlExtender` en la página se debe generar.</span><span class="sxs-lookup"><span data-stu-id="6c09b-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="6c09b-124">El Kit de herramientas de Control de AJAX de ASP.NET ofrece el `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="6c09b-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="6c09b-125">El objeto que devuelve este método es compatible con la `Commit()` método que envía un valor al control que desencadena el menú emergente (no el control que desencadenó la llamada al método!).</span><span class="sxs-lookup"><span data-stu-id="6c09b-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="6c09b-126">El código siguiente proporciona la fecha seleccionada como argumento para el `Commit()` método, haciendo que el código escribir la fecha seleccionada en el cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="6c09b-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="6c09b-127">Al hacer clic en una fecha del calendario, la fecha seleccionada aparece en el cuadro de texto asociado, crear un control de selector de fecha que puede actualmente se encuentra ahora en muchos sitios Web.</span><span class="sxs-lookup"><span data-stu-id="6c09b-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="6c09b-128">[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6c09b-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="6c09b-129">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6c09b-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="6c09b-130">[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6c09b-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="6c09b-131">Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6c09b-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6c09b-132">[Anterior](using-multiple-popup-controls-vb.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6c09b-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
