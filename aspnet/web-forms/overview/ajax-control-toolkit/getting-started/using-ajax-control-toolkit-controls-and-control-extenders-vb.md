---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Uso de los controles de AJAX Control Toolkit y los controles extensores (VB) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 292a6e41740538d41e26b01fa76ff59f9af21221
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825365"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="9976f-103">Uso de los controles de AJAX Control Toolkit y los controles extensores (VB)</span><span class="sxs-lookup"><span data-stu-id="9976f-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="9976f-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9976f-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9976f-105">Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9976f-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="9976f-106">AJAX Control Toolkit contiene un conjunto de controles y extensores de control.</span><span class="sxs-lookup"><span data-stu-id="9976f-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="9976f-107">En este breve tutorial, obtendrá información sobre cómo agregar controles y extensores de control a una página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9976f-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9976f-108">Para obtener instrucciones sobre cómo instalar AJAX Control Toolkit y adición de AJAX Control Toolkit al cuadro de herramientas de Visual Studio o Visual Web Developer, vea el tutorial [Introducción a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="9976f-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="9976f-109">Uso de los controles de AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="9976f-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="9976f-110">Un control de AJAX Control Toolkit funciona igual que un control ASP.NET normal.</span><span class="sxs-lookup"><span data-stu-id="9976f-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="9976f-111">Puede arrastrar el control de cuadro de herramientas a una página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9976f-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="9976f-112">Puede agregar el control a la página en la vista Diseño o vista del origen.</span><span class="sxs-lookup"><span data-stu-id="9976f-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="9976f-113">Hay uno de los requisitos especial al utilizar los controles de AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9976f-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="9976f-114">La página debe contener un control ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="9976f-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="9976f-115">El control ScriptManager es responsable de incluir todo el código JavaScript necesario requerido por los controles de AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="9976f-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="9976f-116">Por ejemplo, la pestaña de AJAX Control Toolkit incluye un control denominado el control del Editor.</span><span class="sxs-lookup"><span data-stu-id="9976f-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="9976f-117">Este control muestra un editor de HTML enriquecido.</span><span class="sxs-lookup"><span data-stu-id="9976f-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="9976f-118">Siga estos pasos para agregar el control del Editor a una página:</span><span class="sxs-lookup"><span data-stu-id="9976f-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="9976f-119">Cree una nueva página ASP.NET denominada ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="9976f-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="9976f-120">Seleccione el control ScriptManager de la pestaña Extensiones de AJAX en el cuadro de herramientas y arrastre el control a la página.</span><span class="sxs-lookup"><span data-stu-id="9976f-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="9976f-121">Seleccione el control del Editor de la pestaña de AJAX Control Toolkit en el cuadro de herramientas y arrastre el control a la página (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="9976f-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="9976f-122">El diseñador debería parecerse a la figura 2.</span><span class="sxs-lookup"><span data-stu-id="9976f-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="9976f-123">Ejecute el sitio web de seleccionando la opción de menú **depurar, Iniciar depuración** o presionar la tecla F5.</span><span class="sxs-lookup"><span data-stu-id="9976f-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="9976f-124">Debería ver la página en la figura 3.</span><span class="sxs-lookup"><span data-stu-id="9976f-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="9976f-125">[![Selecciona el control de Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="9976f-126">**Figura 01**: selecciona el control de Editor HTML ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="9976f-127">[![Diseñador de Visual Studio con el control ScriptManager y edición](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="9976f-128">**Figura 02**: Diseñador de Visual Studio con el control ScriptManager y edición ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="9976f-129">[![La página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="9976f-130">**Figura 03**: DisplayEditor.aspx la página ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="9976f-131">Uso de los extensores de AJAX Control Toolkit Control</span><span class="sxs-lookup"><span data-stu-id="9976f-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="9976f-132">El Kit de herramientas de Control de AJAX también contiene los extensores de control.</span><span class="sxs-lookup"><span data-stu-id="9976f-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="9976f-133">Como sugiere su nombre, un extensor de control amplía la funcionalidad de un control existente.</span><span class="sxs-lookup"><span data-stu-id="9976f-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="9976f-134">Por ejemplo, el extensor de control ConfirmButton amplía el control de botón de ASP.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="9976f-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="9976f-135">El extensor cambia el comportamiento de s de control de botón para que el botón muestra un cuadro de diálogo de confirmación cuando haga clic en él.</span><span class="sxs-lookup"><span data-stu-id="9976f-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="9976f-136">Un extensor de control, al igual que un control de AJAX Control Toolkit, requiere un control ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="9976f-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="9976f-137">Debe agregar un control ScriptManager a una página para empezar a usar los controles extensores en la página.</span><span class="sxs-lookup"><span data-stu-id="9976f-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="9976f-138">Siga estos pasos para usar el extensor de control ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="9976f-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="9976f-139">Cree una nueva página ASP.NET denominada ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="9976f-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="9976f-140">Agregue un control ScriptManager en la página, arrastre el control a la página de la pestaña Extensiones de AJAX.</span><span class="sxs-lookup"><span data-stu-id="9976f-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="9976f-141">Agregue un control de botón estándar a la página arrastrando el botón de la ficha estándar en el cuadro de herramientas hasta la superficie del diseñador.</span><span class="sxs-lookup"><span data-stu-id="9976f-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="9976f-142">Haga clic en el **Agregar extensor** opción de tarea (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="9976f-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="9976f-143">En el cuadro de diálogo Elegir extensor, seleccione ConfirmButtonExtender (consulte la figura 5) y haga clic en el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="9976f-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="9976f-144">Seleccione el control de botón en el diseñador y expanda los extensores, Button1\_ConfirmButtonExtender nodo en la ventana Propiedades (consulte la figura 6).</span><span class="sxs-lookup"><span data-stu-id="9976f-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="9976f-145">Asigne el valor *realmente?* a la propiedad ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="9976f-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="9976f-146">Ejecute la página seleccionando la opción de menú **depurar, Iniciar depuración** o presione la tecla F5.</span><span class="sxs-lookup"><span data-stu-id="9976f-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="9976f-147">[![La opción de la tarea de agregar Extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="9976f-148">**Figura 04**: opción de la tarea de agregar el extensor ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="9976f-149">[![Seleccionar el extensor ConfirmButton de control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="9976f-150">**Figura 05**: seleccionar el extensor de control ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="9976f-151">[![Establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="9976f-152">**Figura 06**: establecer una propiedad ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="9976f-153">Cuando se abre la página, verá un botón.</span><span class="sxs-lookup"><span data-stu-id="9976f-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="9976f-154">Al hacer clic en el botón, obtenga el cuadro de diálogo de confirmación en la figura 7.</span><span class="sxs-lookup"><span data-stu-id="9976f-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="9976f-155">[![Mostrar el cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="9976f-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="9976f-156">**Figura 07**: mostrar el cuadro de diálogo de confirmación ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="9976f-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="9976f-157">Tenga en cuenta que normalmente no arrastre un extensor de control a una página.</span><span class="sxs-lookup"><span data-stu-id="9976f-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="9976f-158">En su lugar, usa el **Agregar extensor** opción para agregar un dispositivo extender a un control que ya ha agregado a una página de tareas.</span><span class="sxs-lookup"><span data-stu-id="9976f-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="9976f-159">Además, tenga en cuenta establece control propiedades extensoras abriendo la hoja de propiedades para el control que se va a extender.</span><span class="sxs-lookup"><span data-stu-id="9976f-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="9976f-160">Un único control ASP.NET puede ampliarse mediante varios extensores de control.</span><span class="sxs-lookup"><span data-stu-id="9976f-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="9976f-161">La hoja de propiedades para el control que se va a extender enumerará todos los extensores de control asociados al control.</span><span class="sxs-lookup"><span data-stu-id="9976f-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9976f-162">[Anterior](get-started-with-the-ajax-control-toolkit-vb.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9976f-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
