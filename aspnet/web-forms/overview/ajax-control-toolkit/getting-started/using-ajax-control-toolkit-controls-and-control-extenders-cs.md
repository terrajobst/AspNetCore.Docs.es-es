---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Uso de los controles de AJAX Control Toolkit y los controles extensores (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 18ee6dd71fe0e84ec7628eba63aabeee0690d0b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837307"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="32067-103">Uso de los controles de AJAX Control Toolkit y los controles extensores (C#)</span><span class="sxs-lookup"><span data-stu-id="32067-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="32067-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="32067-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="32067-105">Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32067-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="32067-106">AJAX Control Toolkit contiene un conjunto de controles y extensores de control.</span><span class="sxs-lookup"><span data-stu-id="32067-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="32067-107">En este breve tutorial, obtendrá información sobre cómo agregar controles y extensores de control a una página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32067-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="32067-108">Para obtener instrucciones sobre cómo instalar AJAX Control Toolkit y adición de AJAX Control Toolkit al cuadro de herramientas de Visual Studio o Visual Web Developer, vea el tutorial [Introducción a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="32067-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="32067-109">Uso de los controles de AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="32067-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="32067-110">Un control de AJAX Control Toolkit funciona igual que un control ASP.NET normal.</span><span class="sxs-lookup"><span data-stu-id="32067-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="32067-111">Puede arrastrar el control de cuadro de herramientas a una página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32067-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="32067-112">Puede agregar el control a la página en la vista Diseño o vista del origen.</span><span class="sxs-lookup"><span data-stu-id="32067-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="32067-113">Hay uno de los requisitos especial al utilizar los controles de AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="32067-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="32067-114">La página debe contener un control ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="32067-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="32067-115">El control ScriptManager es responsable de incluir todo el código JavaScript necesario requerido por los controles de AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="32067-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="32067-116">Por ejemplo, la pestaña de AJAX Control Toolkit incluye un control denominado el control del Editor.</span><span class="sxs-lookup"><span data-stu-id="32067-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="32067-117">Este control muestra un editor de HTML enriquecido.</span><span class="sxs-lookup"><span data-stu-id="32067-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="32067-118">Siga estos pasos para agregar el control del Editor a una página:</span><span class="sxs-lookup"><span data-stu-id="32067-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="32067-119">Cree una nueva página ASP.NET denominada ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="32067-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="32067-120">Seleccione el control ScriptManager de la pestaña Extensiones de AJAX en el cuadro de herramientas y arrastre el control a la página.</span><span class="sxs-lookup"><span data-stu-id="32067-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="32067-121">Seleccione el control del Editor de la pestaña de AJAX Control Toolkit en el cuadro de herramientas y arrastre el control a la página (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="32067-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="32067-122">El diseñador debería parecerse a la figura 2.</span><span class="sxs-lookup"><span data-stu-id="32067-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="32067-123">Ejecute el sitio web de seleccionando la opción de menú **depurar, Iniciar depuración** o presionar la tecla F5.</span><span class="sxs-lookup"><span data-stu-id="32067-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="32067-124">Debería ver la página en la figura 3.</span><span class="sxs-lookup"><span data-stu-id="32067-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="32067-125">[![Selecciona el control de Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32067-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="32067-126">**Figura 01**: selecciona el control de Editor HTML ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="32067-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="32067-127">[![Diseñador de Visual Studio con el control ScriptManager y edición](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="32067-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="32067-128">**Figura 02**: Diseñador de Visual Studio con el control ScriptManager y edición ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="32067-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="32067-129">[![La página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="32067-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="32067-130">**Figura 03**: DisplayEditor.aspx la página ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="32067-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="32067-131">Uso de los extensores de AJAX Control Toolkit Control</span><span class="sxs-lookup"><span data-stu-id="32067-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="32067-132">El Kit de herramientas de Control de AJAX también contiene los extensores de control.</span><span class="sxs-lookup"><span data-stu-id="32067-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="32067-133">Como sugiere su nombre, un extensor de control amplía la funcionalidad de un control existente.</span><span class="sxs-lookup"><span data-stu-id="32067-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="32067-134">Por ejemplo, el extensor de control ConfirmButton amplía el control de botón de ASP.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="32067-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="32067-135">El extensor cambia el comportamiento de s de control de botón para que el botón muestra un cuadro de diálogo de confirmación cuando haga clic en él.</span><span class="sxs-lookup"><span data-stu-id="32067-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="32067-136">Un extensor de control, al igual que un control de AJAX Control Toolkit, requiere un control ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="32067-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="32067-137">Debe agregar un control ScriptManager a una página para empezar a usar los controles extensores en la página.</span><span class="sxs-lookup"><span data-stu-id="32067-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="32067-138">Siga estos pasos para usar el extensor de control ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="32067-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="32067-139">Cree una nueva página ASP.NET denominada ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="32067-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="32067-140">Agregue un control ScriptManager en la página, arrastre el control a la página de la pestaña Extensiones de AJAX.</span><span class="sxs-lookup"><span data-stu-id="32067-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="32067-141">Agregue un control de botón estándar a la página arrastrando el botón de la ficha estándar en el cuadro de herramientas hasta la superficie del diseñador.</span><span class="sxs-lookup"><span data-stu-id="32067-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="32067-142">Haga clic en el **Agregar extensor** opción de tarea (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="32067-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="32067-143">En el cuadro de diálogo Elegir extensor, seleccione ConfirmButtonExtender (consulte la figura 5) y haga clic en el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="32067-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="32067-144">Seleccione el control de botón en el diseñador y expanda los extensores, Button1\_ConfirmButtonExtender nodo en la ventana Propiedades (consulte la figura 6).</span><span class="sxs-lookup"><span data-stu-id="32067-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="32067-145">Asigne el valor *realmente?* a la propiedad ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="32067-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="32067-146">Ejecute la página seleccionando la opción de menú **depurar, Iniciar depuración** o presione la tecla F5.</span><span class="sxs-lookup"><span data-stu-id="32067-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="32067-147">[![La opción de la tarea de agregar Extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="32067-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="32067-148">**Figura 04**: opción de la tarea de agregar el extensor ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="32067-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="32067-149">[![Seleccionar el extensor ConfirmButton de control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="32067-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="32067-150">**Figura 05**: seleccionar el extensor de control ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="32067-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="32067-151">[![Establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="32067-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="32067-152">**Figura 06**: establecer una propiedad ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="32067-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="32067-153">Cuando se abre la página, verá un botón.</span><span class="sxs-lookup"><span data-stu-id="32067-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="32067-154">Al hacer clic en el botón, obtenga el cuadro de diálogo de confirmación en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="32067-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="32067-155">[![Mostrar el cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="32067-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="32067-156">**Figura 07**: mostrar el cuadro de diálogo de confirmación ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="32067-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="32067-157">Tenga en cuenta que normalmente no arrastre un extensor de control a una página.</span><span class="sxs-lookup"><span data-stu-id="32067-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="32067-158">En su lugar, usa el **Agregar extensor** opción para agregar un dispositivo extender a un control que ya ha agregado a una página de tareas.</span><span class="sxs-lookup"><span data-stu-id="32067-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="32067-159">Además, tenga en cuenta establece control propiedades extensoras abriendo la hoja de propiedades para el control que se va a extender.</span><span class="sxs-lookup"><span data-stu-id="32067-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="32067-160">Un único control ASP.NET puede ampliarse mediante varios extensores de control.</span><span class="sxs-lookup"><span data-stu-id="32067-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="32067-161">La hoja de propiedades para el control que se va a extender enumerará todos los extensores de control asociados al control.</span><span class="sxs-lookup"><span data-stu-id="32067-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32067-162">[Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="32067-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
