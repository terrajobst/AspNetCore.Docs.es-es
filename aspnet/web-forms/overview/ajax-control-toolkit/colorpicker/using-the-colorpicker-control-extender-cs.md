---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Usar el extensor del Control ColorPicker (C#) | Microsoft Docs
author: microsoft
description: ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente. Se puede conectar a cualquier ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 58b8d581ed426227ed77435e22c84e9ea5d62ebe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836998"
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="f9be3-104">Usar el extensor del Control ColorPicker (C#)</span><span class="sxs-lookup"><span data-stu-id="f9be3-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="f9be3-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f9be3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f9be3-106">ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control emergente.</span><span class="sxs-lookup"><span data-stu-id="f9be3-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="f9be3-107">Se puede conectar a cualquier control de cuadro de texto de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f9be3-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="f9be3-108">Él.</span><span class="sxs-lookup"><span data-stu-id="f9be3-108">It.</span></span>


<span data-ttu-id="f9be3-109">El objetivo de este tutorial es explicar cómo puede usar el extensor de control de AJAX Control Toolkit ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="f9be3-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="f9be3-110">El extensor de control ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="f9be3-111">ColorPicker es útil cuando desea proporcionar una interfaz de usuario intuitiva para un usuario elegir un color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="f9be3-112">Ampliación de un Control de cuadro de texto con el extensor del Control ColorPicker</span><span class="sxs-lookup"><span data-stu-id="f9be3-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="f9be3-113">Por ejemplo, imagine que desea crear un sitio Web que permite a los visitantes crear tarjetas de presentación personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f9be3-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="f9be3-114">Los visitantes pueden escribir el texto para una tarjeta de presentación y seleccionar el color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="f9be3-115">La página ASP.NET en el listado 1 contiene dos controles TextBox denominados txtCardText y txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="f9be3-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="f9be3-116">Cuando se envía el formulario, se muestran los valores seleccionados (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="f9be3-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="f9be3-117">[![Formulario simple para crear una tarjeta de presentación](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f9be3-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="f9be3-118">**Figura 01**: un formulario Simple para crear una tarjeta de presentación ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f9be3-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="f9be3-119">**Listado 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="f9be3-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="f9be3-120">El formulario en el listado 1 funciona, pero no proporciona una experiencia de usuario excelente.</span><span class="sxs-lookup"><span data-stu-id="f9be3-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="f9be3-121">El usuario debe escribir un color en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="f9be3-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="f9be3-122">Si el usuario desea un color especializado - por ejemplo, solo el derecho tono verde pea -, a continuación, el usuario debe averiguar el código de color HTML sin ayuda.</span><span class="sxs-lookup"><span data-stu-id="f9be3-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="f9be3-123">Puede usar el extensor de control ColorPicker para crear una mejor experiencia de usuario.</span><span class="sxs-lookup"><span data-stu-id="f9be3-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="f9be3-124">ColorPicker muestra un cuadro de diálogo color al mover el foco a un control de cuadro de texto (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="f9be3-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="f9be3-125">[![El extensor del Control ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f9be3-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="f9be3-126">**Figura 02**: el extensor del Control ColorPicker ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f9be3-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="f9be3-127">Es preciso realizar dos pasos para usar el extensor de control ColorPicker con el formulario en el listado 1:</span><span class="sxs-lookup"><span data-stu-id="f9be3-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="f9be3-128">Agregar un control ScriptManager a la página</span><span class="sxs-lookup"><span data-stu-id="f9be3-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="f9be3-129">Agregue el extensor de control ColorPicker a la página</span><span class="sxs-lookup"><span data-stu-id="f9be3-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="f9be3-130">Para poder usar ColorPicker, debe agregar un ScriptManager en la página.</span><span class="sxs-lookup"><span data-stu-id="f9be3-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="f9be3-131">Es un buen lugar para agregar el control ScriptManager justo debajo de la apertura del servidor &lt;formulario&gt; etiqueta.</span><span class="sxs-lookup"><span data-stu-id="f9be3-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="f9be3-132">Puede arrastrar el control ScriptManager hasta la página del cuadro de herramientas (ScriptManager se encuentra en la pestaña Extensiones de AJAX).</span><span class="sxs-lookup"><span data-stu-id="f9be3-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="f9be3-133">Como alternativa, puede escribir la siguiente etiqueta en la vista del origen de debajo de la etiqueta de formulario de servidor de apertura:</span><span class="sxs-lookup"><span data-stu-id="f9be3-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="f9be3-134">&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="f9be3-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="f9be3-135">Es la manera más fácil de agregar el extensor de control ColorPicker a la página en la vista Diseño.</span><span class="sxs-lookup"><span data-stu-id="f9be3-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="f9be3-136">Si se mantenga el mouse sobre el cuadro de texto txtCardColor, una opción de tarea inteligente aparece permite agregar un extensor (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="f9be3-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="f9be3-137">Si elige esta opción, aparece el Asistente de extensor (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="f9be3-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="f9be3-138">[![Adición de un extensor](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f9be3-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="f9be3-139">**Figura 03**: adición de un extensor ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f9be3-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="f9be3-140">[![Seleccionar un extensor de control con el Asistente para Extender](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f9be3-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="f9be3-141">**Figura 04**: selección de un extensor de control con el Asistente para Extender ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f9be3-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="f9be3-142">Puede elegir el extensor ColorPicker para ampliar el cuadro de texto txtCardColor con el extensor ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="f9be3-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="f9be3-143">Haga clic en Aceptar para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="f9be3-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="f9be3-144">Después de realizar estos cambios, el origen de la página parece listado 2.</span><span class="sxs-lookup"><span data-stu-id="f9be3-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="f9be3-145">Listado 2 - CreateCard.aspx (con ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="f9be3-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="f9be3-146">Tenga en cuenta que la página ahora contiene un control ColorPickerExtender que aparece justo debajo del control TextBox txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="f9be3-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="f9be3-147">El control ColorPickerExtender extiende el control de txtCardColor para que se muestre un cuadro de diálogo del selector de color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="f9be3-148">Usar un botón para iniciar el cuadro de diálogo del selector de Color</span><span class="sxs-lookup"><span data-stu-id="f9be3-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="f9be3-149">El extensor ColorPicker admite las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="f9be3-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="f9be3-150">PopupButtonId - el identificador de un botón en la página que hace que el cuadro de diálogo del selector de color que aparezca.</span><span class="sxs-lookup"><span data-stu-id="f9be3-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="f9be3-151">PopupPosition: la posición en relación con el control de destino, del cuadro de diálogo de selector de color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="f9be3-152">Los valores posibles son absolutos, centro, BottomLeft, BottomRight, TopLeft, superior, derecha e izquierda (el valor predeterminado es BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="f9be3-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="f9be3-153">SampleControlId - el identificador de un control que muestra el color seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f9be3-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="f9be3-154">SelectedColor - color seleccionado por el componente ColorPicker inicial.</span><span class="sxs-lookup"><span data-stu-id="f9be3-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="f9be3-155">Puede usar estas propiedades para personalizar cómo se muestra el cuadro de diálogo del selector de color y cómo se muestra el color seleccionado.</span><span class="sxs-lookup"><span data-stu-id="f9be3-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="f9be3-156">La página en el listado 3 muestra cómo puede usar varias de estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="f9be3-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="f9be3-157">**Listado 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="f9be3-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="f9be3-158">La página en el listado 3 incluye seleccionar un Color (consulte la figura 5).</span><span class="sxs-lookup"><span data-stu-id="f9be3-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="f9be3-159">Al hacer clic en este botón, aparece el cuadro de diálogo del selector de color por encima del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="f9be3-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="f9be3-160">Si selecciona un color en el cuadro de diálogo color seleccionado aparece como el color de fondo de la lblSample control de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="f9be3-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="f9be3-161">La propiedad ColorPicker PopupButtonID se utiliza para asociar el extensor ColorPicker en el botón Seleccionar Color.</span><span class="sxs-lookup"><span data-stu-id="f9be3-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="f9be3-162">Al proporcionar un valor para la propiedad PopupButtonID, el cuadro de diálogo del selector de color ya no aparece cuando el control de destino tiene el foco.</span><span class="sxs-lookup"><span data-stu-id="f9be3-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="f9be3-163">Debe hacer clic en el botón para mostrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="f9be3-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="f9be3-164">La propiedad SampleControlID se utiliza para asociar un control que muestra el color seleccionado con ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="f9be3-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="f9be3-165">ColorPicker cambia el color de fondo de este control para el color seleccionado actualmente.</span><span class="sxs-lookup"><span data-stu-id="f9be3-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="f9be3-166">[![Mostrar el cuadro de diálogo del selector de color con un botón](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f9be3-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="f9be3-167">**Figura 05**: mostrar el cuadro de diálogo del selector de color con un botón ([haga clic aquí para ver imagen en tamaño completo](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f9be3-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="f9be3-168">Resumen</span><span class="sxs-lookup"><span data-stu-id="f9be3-168">Summary</span></span>

<span data-ttu-id="f9be3-169">En este tutorial, ha aprendido a usar el extensor de control ColorPicker para mostrar un cuadro de diálogo de selector de colores emergente.</span><span class="sxs-lookup"><span data-stu-id="f9be3-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="f9be3-170">En primer lugar, hemos visto cómo se puede mostrar el cuadro de diálogo cuando se mueve el foco a un control de cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="f9be3-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="f9be3-171">A continuación, ha aprendido a crear un botón que muestra el cuadro de diálogo del selector de color cuando se hace clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="f9be3-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f9be3-172">Siguiente</span><span class="sxs-lookup"><span data-stu-id="f9be3-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
