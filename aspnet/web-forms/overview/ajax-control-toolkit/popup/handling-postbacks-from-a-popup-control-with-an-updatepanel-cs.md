---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Controlar los Postbacks desde un Control emergente con un UpdatePanel (C#) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Debe tenerse especial cuidado...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9fdf2821e005bfcb691134bb6994c01fa72b7abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815387"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="02b28-104">Controlar los Postbacks desde un Control emergente con un UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="02b28-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="02b28-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02b28-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02b28-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="02b28-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="02b28-107">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="02b28-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="02b28-108">Tiene un cuidado especial que se realizará cuando se produce un postback dentro de este tipo una ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="02b28-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="02b28-109">Información general</span><span class="sxs-lookup"><span data-stu-id="02b28-109">Overview</span></span>

<span data-ttu-id="02b28-110">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="02b28-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="02b28-111">Tiene un cuidado especial que se realizará cuando se produce un postback dentro de este tipo una ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="02b28-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="02b28-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="02b28-112">Steps</span></span>

<span data-ttu-id="02b28-113">Cuando se usa un `PopupControl` con una devolución de datos, un `UpdatePanel` puede evitar que la actualización de la página causada por la devolución de datos.</span><span class="sxs-lookup"><span data-stu-id="02b28-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="02b28-114">El marcado siguiente define un par de elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="02b28-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="02b28-115">Un `ScriptManager` controlar para que funcione de ASP.NET AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="02b28-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="02b28-116">Dos `TextBox` controla qué ambos, se desencadenará un menú emergente</span><span class="sxs-lookup"><span data-stu-id="02b28-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="02b28-117">Un `Panel` control que actúa como el elemento emergente</span><span class="sxs-lookup"><span data-stu-id="02b28-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="02b28-118">Dentro del panel, un `Calendar` control incrustado en un `UpdatePanel` control</span><span class="sxs-lookup"><span data-stu-id="02b28-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="02b28-119">Dos `PopupControlExtender` controles que asignación el panel a los cuadros de texto</span><span class="sxs-lookup"><span data-stu-id="02b28-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="02b28-120">Tenga en cuenta que el `OnSelectionChanged` atributo de la `Calendar` se establece el control.</span><span class="sxs-lookup"><span data-stu-id="02b28-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="02b28-121">Por lo que cuando el usuario selecciona una fecha del calendario, se produce un postback y el método de servidor `c1_SelectionChanged()` se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="02b28-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="02b28-122">Dentro de ese método, se debe recuperar la fecha actual y volver a escribir en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="02b28-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="02b28-123">La sintaxis que es como sigue: en primer lugar, un proxy de objetos para el `PopupControlExtender` debe haberse generado en la página.</span><span class="sxs-lookup"><span data-stu-id="02b28-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="02b28-124">ASP.NET AJAX Control Toolkit ofrece el `GetProxyForCurrentPopup()` método.</span><span class="sxs-lookup"><span data-stu-id="02b28-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="02b28-125">El objeto que devuelve este método admite la `Commit()` método que envía un valor al control que desencadena la ventana emergente (no el control que desencadenó la llamada al método!).</span><span class="sxs-lookup"><span data-stu-id="02b28-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="02b28-126">El código siguiente proporciona la fecha seleccionada como argumento para el `Commit()` método, lo que provocaría el código escribir la fecha seleccionada en el cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="02b28-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="02b28-127">Ahora al hacer clic en una fecha del calendario, la fecha seleccionada aparece en el cuadro de texto asociado, crear un control de selector de fecha que puede actualmente se encuentra en muchos sitios Web.</span><span class="sxs-lookup"><span data-stu-id="02b28-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="02b28-128">[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02b28-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="02b28-129">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02b28-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="02b28-130">[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="02b28-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="02b28-131">Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="02b28-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02b28-132">[Anterior](using-multiple-popup-controls-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="02b28-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
