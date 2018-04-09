---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Usar el extensor de Control de ColorPicker (C#) | Documentos de Microsoft
author: microsoft
description: ColorPicker es un extensor de AJAX de ASP.NET que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control de menú emergente. Se puede adjuntar a cualquier ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d44fc81305e668b545246cf044dce275563d81a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="2946f-104">Usar el extensor de Control de ColorPicker (C#)</span><span class="sxs-lookup"><span data-stu-id="2946f-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="2946f-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2946f-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="2946f-106">ColorPicker es un extensor de AJAX de ASP.NET que proporciona funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control de menú emergente.</span><span class="sxs-lookup"><span data-stu-id="2946f-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="2946f-107">Se puede adjuntar a cualquier control de cuadro de texto de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2946f-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="2946f-108">Se.</span><span class="sxs-lookup"><span data-stu-id="2946f-108">It.</span></span>


<span data-ttu-id="2946f-109">El objetivo de este tutorial es explicar cómo se puede utilizar el extensor de control AJAX Control Toolkit ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="2946f-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="2946f-110">El extensor de control de ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color.</span><span class="sxs-lookup"><span data-stu-id="2946f-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="2946f-111">El componente ColorPicker es útil cuando desea proporcionar una interfaz de usuario intuitiva para que un usuario elige un color.</span><span class="sxs-lookup"><span data-stu-id="2946f-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="2946f-112">Extender un Control de cuadro de texto con el extensor de Control de ColorPicker</span><span class="sxs-lookup"><span data-stu-id="2946f-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="2946f-113">Por ejemplo, imagine que desea crear un sitio Web que permite a los visitantes crear tarjetas de presentación personalizadas.</span><span class="sxs-lookup"><span data-stu-id="2946f-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="2946f-114">Los visitantes pueden escribir el texto para una tarjeta de presentación y seleccionar el color.</span><span class="sxs-lookup"><span data-stu-id="2946f-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="2946f-115">La página ASP.NET en el listado 1 contiene dos controles de cuadro de texto denominado txtCardText y txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="2946f-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="2946f-116">Cuando se envía el formulario, se muestran los valores seleccionados (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="2946f-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="2946f-117">[![Forma sencilla para crear una tarjeta de presentación](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2946f-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="2946f-118">**Figura 01**: formulario Simple para crear una tarjeta de presentación ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2946f-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="2946f-119">**Lista 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="2946f-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="2946f-120">El formulario en el listado 1 funciona, pero no proporciona una experiencia óptima al usuario.</span><span class="sxs-lookup"><span data-stu-id="2946f-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="2946f-121">El usuario debe escribir un color en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="2946f-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="2946f-122">Si el usuario desea un color especializado - por ejemplo, solo el derecho tonalidad de verde pea -, a continuación, el usuario debe averiguar el código de color HTML sin ningún tipo de ayuda.</span><span class="sxs-lookup"><span data-stu-id="2946f-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="2946f-123">Puede utilizar el extensor de control de ColorPicker para crear una mejor experiencia de usuario.</span><span class="sxs-lookup"><span data-stu-id="2946f-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="2946f-124">El componente ColorPicker muestra un cuadro de diálogo color al mover el foco a un control de cuadro de texto (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="2946f-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="2946f-125">[![El extensor de Control de ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2946f-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="2946f-126">**Figura 02**: el extensor de Control de ColorPicker ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="2946f-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="2946f-127">Debe completar dos pasos para usar el extensor de control ColorPicker con el formulario en el listado 1:</span><span class="sxs-lookup"><span data-stu-id="2946f-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="2946f-128">Agregar un control ScriptManager a la página</span><span class="sxs-lookup"><span data-stu-id="2946f-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="2946f-129">Agregue el extensor de control de ColorPicker a la página</span><span class="sxs-lookup"><span data-stu-id="2946f-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="2946f-130">Para poder usar el componente ColorPicker, debe agregar un ScriptManager a la página.</span><span class="sxs-lookup"><span data-stu-id="2946f-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="2946f-131">Es un buen lugar para agregar el objeto ScriptManager justo debajo del servidor de apertura &lt;formulario&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="2946f-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="2946f-132">Puede arrastrar el objeto ScriptManager a la página del cuadro de herramientas (el objeto ScriptManager se encuentra en la ficha Extensiones AJAX).</span><span class="sxs-lookup"><span data-stu-id="2946f-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="2946f-133">Como alternativa, puede escribir la siguiente etiqueta en la vista del origen de debajo de la etiqueta de formulario de servidor de apertura:</span><span class="sxs-lookup"><span data-stu-id="2946f-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="2946f-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="2946f-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="2946f-135">Es la manera más fácil de agregar el extensor de control de ColorPicker a la página en la vista Diseño.</span><span class="sxs-lookup"><span data-stu-id="2946f-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="2946f-136">Si se mantenga el mouse sobre el cuadro de texto txtCardColor, una opción de tarea inteligente aparece permite agregar un dispositivo extender (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="2946f-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="2946f-137">Si elige esta opción, aparece el Asistente de Extender (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="2946f-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="2946f-138">[![Agregar un dispositivo extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2946f-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="2946f-139">**Figura 03**: agregar un dispositivo extender ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2946f-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="2946f-140">[![Al seleccionar un extensor de control con el Asistente para Extender](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2946f-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="2946f-141">**Figura 04**: seleccionar un extensor de control con el Asistente para Extender ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="2946f-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="2946f-142">Puede elegir el extensor de ColorPicker para ampliar el cuadro de texto txtCardColor con el dispositivo extender ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="2946f-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="2946f-143">Haga clic en Aceptar para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="2946f-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="2946f-144">Después de realizar estos cambios, el origen de la página parece listado 2.</span><span class="sxs-lookup"><span data-stu-id="2946f-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="2946f-145">La lista 2 - CreateCard.aspx (con ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="2946f-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="2946f-146">Observe que la página contiene ahora un control ColorPickerExtender que aparece directamente debajo de la txtCardColor TextBox (control).</span><span class="sxs-lookup"><span data-stu-id="2946f-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="2946f-147">El control ColorPickerExtender amplía el control de txtCardColor para que se muestre un cuadro de diálogo de selector de color.</span><span class="sxs-lookup"><span data-stu-id="2946f-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="2946f-148">Usar un botón para iniciar el cuadro de diálogo de selector de Color</span><span class="sxs-lookup"><span data-stu-id="2946f-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="2946f-149">El extensor de ColorPicker admite las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="2946f-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="2946f-150">PopupButtonId: el identificador de un botón en la página que hace que el cuadro de diálogo de selector de color que aparezca.</span><span class="sxs-lookup"><span data-stu-id="2946f-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="2946f-151">PopupPosition: la posición en relación con el control de destino, del cuadro de diálogo de selector de color.</span><span class="sxs-lookup"><span data-stu-id="2946f-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="2946f-152">Valores posibles son absolutos, centro, BottomLeft, BottomRight, TopLeft, TopRight, derecha e izquierda (el valor predeterminado es BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="2946f-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="2946f-153">SampleControlId: el identificador de un control que muestra el color seleccionado.</span><span class="sxs-lookup"><span data-stu-id="2946f-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="2946f-154">SelectedColor - el color inicial seleccionado por el componente ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="2946f-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="2946f-155">Puede usar estas propiedades para personalizar cómo se muestra el cuadro de diálogo de selector de color y cómo se muestra el color seleccionado.</span><span class="sxs-lookup"><span data-stu-id="2946f-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="2946f-156">La página en la lista 3 muestra cómo puede usar algunas de estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="2946f-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="2946f-157">**El listado 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="2946f-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="2946f-158">La página en la lista 3 incluye un seleccionar Color botón (Véase la figura 5).</span><span class="sxs-lookup"><span data-stu-id="2946f-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="2946f-159">Al hacer clic en este botón, aparece el cuadro de diálogo de selector de color sobre el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="2946f-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="2946f-160">Si selecciona un color en el cuadro de diálogo del color seleccionado aparece como el color de fondo de la lblSample Label (control).</span><span class="sxs-lookup"><span data-stu-id="2946f-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="2946f-161">La propiedad ColorPicker PopupButtonID se utiliza para asociar el dispositivo extender ColorPicker en el botón Seleccionar Color.</span><span class="sxs-lookup"><span data-stu-id="2946f-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="2946f-162">Al proporcionar un valor para la propiedad PopupButtonID, el cuadro de diálogo de selector de color ya no aparece cuando el control de destino tiene el foco.</span><span class="sxs-lookup"><span data-stu-id="2946f-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="2946f-163">Debe hacer clic en el botón para mostrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="2946f-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="2946f-164">La propiedad SampleControlID se utiliza para asociar un control que muestra el color seleccionado con el componente ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="2946f-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="2946f-165">El componente ColorPicker cambia el color de fondo de este control para el color seleccionado actualmente.</span><span class="sxs-lookup"><span data-stu-id="2946f-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="2946f-166">[![Mostrar el cuadro de diálogo de selector de color con un botón](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="2946f-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="2946f-167">**Figura 05**: mostrar el cuadro de diálogo de selector de color con un botón ([haga clic aquí para ver la imagen a tamaño completo](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="2946f-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="2946f-168">Resumen</span><span class="sxs-lookup"><span data-stu-id="2946f-168">Summary</span></span>

<span data-ttu-id="2946f-169">En este tutorial, aprendió a utilizar el extensor de control de ColorPicker para mostrar un cuadro de diálogo de selector de colores emergente.</span><span class="sxs-lookup"><span data-stu-id="2946f-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="2946f-170">En primer lugar, se examina cómo puede mostrar el cuadro de diálogo cuando se mueve el foco a un control de cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="2946f-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="2946f-171">A continuación, aprendió a crear un botón que muestra el cuadro de diálogo de selector de color cuando se hace clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="2946f-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2946f-172">Siguiente</span><span class="sxs-lookup"><span data-stu-id="2946f-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
