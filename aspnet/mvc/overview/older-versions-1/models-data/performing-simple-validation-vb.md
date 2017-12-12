---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: "Realizar una validación Simple (VB) | Documentos de Microsoft"
author: StephenWalther
description: "Obtenga información acerca de cómo realizar la validación en una aplicación ASP.NET MVC. En este tutorial, Stephen Walther presenta se usa para modelar el estado y la aplicación auxiliar validation HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bc4cdbcd267bcdd3e71abc4c52664ae62c5c02e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="performing-simple-validation-vb"></a><span data-ttu-id="350fa-104">Realizar una validación Simple (VB)</span><span class="sxs-lookup"><span data-stu-id="350fa-104">Performing Simple Validation (VB)</span></span>
====================
<span data-ttu-id="350fa-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="350fa-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="350fa-106">Obtenga información acerca de cómo realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="350fa-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="350fa-107">En este tutorial, Stephen Walther presenta se usa para modelar el estado y las aplicaciones auxiliares HTML de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="350fa-108">El objetivo de este tutorial es explicar cómo puede realizar la validación dentro de una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="350fa-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="350fa-109">Por ejemplo, aprenderá cómo impedir que alguien enviar un formulario que no contiene un valor para un campo obligatorio.</span><span class="sxs-lookup"><span data-stu-id="350fa-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="350fa-110">Obtenga información acerca de cómo usar el estado del modelo y las aplicaciones auxiliares HTML de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="350fa-111">Estado del modelo de descripción</span><span class="sxs-lookup"><span data-stu-id="350fa-111">Understanding Model State</span></span>

<span data-ttu-id="350fa-112">Usar estado del modelo - o más concretamente, el diccionario de Estados del modelo - para representar errores de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="350fa-113">Por ejemplo, la acción Create() en el listado 1 valida las propiedades de una clase de producto antes de agregar la clase de producto a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="350fa-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="350fa-114">No estoy recomendación de que se agrega la lógica de validación o la base de datos a un controlador.</span><span class="sxs-lookup"><span data-stu-id="350fa-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="350fa-115">Un controlador debe contener solo lógica relacionada con el control de flujo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="350fa-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="350fa-116">Nos quedamos con un acceso directo para no complicar las cosas.</span><span class="sxs-lookup"><span data-stu-id="350fa-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="350fa-117">**Lista 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="350fa-117">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

<span data-ttu-id="350fa-118">En la lista 1, se validan las propiedades nombre, descripción y UnitsInStock de la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="350fa-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="350fa-119">Si cualquiera de estas propiedades producirá un error en una prueba de validación se agrega un error en el diccionario de Estados del modelo (representado por la propiedad ModelState de la clase de controlador).</span><span class="sxs-lookup"><span data-stu-id="350fa-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="350fa-120">Si hay errores en el estado del modelo, a continuación, la propiedad ModelState.IsValid devuelve false.</span><span class="sxs-lookup"><span data-stu-id="350fa-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="350fa-121">En ese caso, se vuelve a mostrar en el formulario HTML para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="350fa-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="350fa-122">En caso contrario, si no hay ningún error de validación, el nuevo producto se agrega a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="350fa-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="350fa-123">Uso de las aplicaciones auxiliares de validación</span><span class="sxs-lookup"><span data-stu-id="350fa-123">Using the Validation Helpers</span></span>

<span data-ttu-id="350fa-124">El marco de ASP.NET MVC incluye dos aplicaciones auxiliares de validación: la aplicación auxiliar Html.ValidationMessage() y la aplicación auxiliar Html.ValidationSummary().</span><span class="sxs-lookup"><span data-stu-id="350fa-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="350fa-125">Usar estas aplicaciones auxiliares de dos en una vista para mostrar mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="350fa-126">Las aplicaciones auxiliares Html.ValidationMessage() y Html.ValidationSummary() se utilizan en las vistas de creación y edición que se generan automáticamente con el scaffolding de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="350fa-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="350fa-127">Siga estos pasos para generar la vista de creación:</span><span class="sxs-lookup"><span data-stu-id="350fa-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="350fa-128">Haga clic en la acción Create() en el controlador de producto y seleccione la opción de menú **agregar vista** (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="350fa-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="350fa-129">En el **agregar vista** cuadro de diálogo, active la casilla etiquetada **crear una vista fuertemente tipada** (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="350fa-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="350fa-130">Desde el **ver datos clase** lista desplegable, seleccione la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="350fa-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="350fa-131">Desde el **ver contenido** lista desplegable, seleccione crear.</span><span class="sxs-lookup"><span data-stu-id="350fa-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="350fa-132">Haga clic en el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="350fa-132">Click the **Add** button.</span></span>


<span data-ttu-id="350fa-133">Asegúrese de que compila la aplicación antes de agregar una vista.</span><span class="sxs-lookup"><span data-stu-id="350fa-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="350fa-134">En caso contrario, no aparecerá la lista de clases en el **ver datos clase** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="350fa-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="350fa-135">[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="350fa-135">[![The New Project dialog box](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)</span></span>

<span data-ttu-id="350fa-136">**Figura 01**: agregar una vista ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="350fa-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-vb/_static/image2.png))</span></span>


<span data-ttu-id="350fa-137">[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="350fa-137">[![The New Project dialog box](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)</span></span>

<span data-ttu-id="350fa-138">**Figura 02**: crear una vista fuertemente tipada ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="350fa-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-vb/_static/image4.png))</span></span>


<span data-ttu-id="350fa-139">Después de completar estos pasos, obtener la vista de creación en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="350fa-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="350fa-140">**La lista 2 - Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="350fa-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

<span data-ttu-id="350fa-141">En el listado 2, la aplicación auxiliar Html.ValidationSummary() se llama inmediatamente encima del formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="350fa-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="350fa-142">Esta aplicación auxiliar se utiliza para mostrar una lista de mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="350fa-143">La aplicación auxiliar Html.ValidationSummary() representa los errores en una lista con viñetas.</span><span class="sxs-lookup"><span data-stu-id="350fa-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="350fa-144">Se llama a la aplicación auxiliar Html.ValidationMessage() junto a cada uno de los campos de formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="350fa-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="350fa-145">Esta aplicación auxiliar se utiliza para mostrar un mensaje de error junto a un campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="350fa-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="350fa-146">En el caso de listado 2, la aplicación auxiliar Html.ValidationMessage() muestra un asterisco cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="350fa-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="350fa-147">La página en la figura 3 muestra los mensajes de error que se representan por las aplicaciones auxiliares de validación cuando se envía el formulario con los campos que faltan y valores no válidos.</span><span class="sxs-lookup"><span data-stu-id="350fa-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="350fa-148">[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="350fa-148">[![The New Project dialog box](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)</span></span>

<span data-ttu-id="350fa-149">**Figura 03**: crear la vista se ha enviado con problemas ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="350fa-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-vb/_static/image6.png))</span></span>


<span data-ttu-id="350fa-150">Tenga en cuenta que también se modifican los campos cuando se produce un error de validación de entrada de la apariencia del HTML.</span><span class="sxs-lookup"><span data-stu-id="350fa-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="350fa-151">La aplicación auxiliar Html.TextBox() se representan una *clase = "error de validación de entrada"* atributo cuando se produce un error de validación asociado con la propiedad representada por la aplicación auxiliar Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="350fa-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="350fa-152">Hay tres clases de hoja de estilos en cascada permiten controlar la apariencia de los errores de validación:</span><span class="sxs-lookup"><span data-stu-id="350fa-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="350fa-153">error de entrada-validación - se aplica a la &lt;entrada&gt; etiqueta representada por la aplicación auxiliar de Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="350fa-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="350fa-154">campo--error de validación - se aplica a la &lt;abarcan&gt; etiqueta representada por la aplicación auxiliar Html.ValidationMessage().</span><span class="sxs-lookup"><span data-stu-id="350fa-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="350fa-155">Resumen-errores de validación: se aplica a la &lt;ul&gt; etiqueta representada por la aplicación auxiliar Html.ValidationSumamry().</span><span class="sxs-lookup"><span data-stu-id="350fa-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="350fa-156">Puede modificar estas clases de hoja de estilos en cascada y, por lo tanto, modificar el aspecto de los errores de validación, modificando el archivo Site.css ubicado en la carpeta de contenido.</span><span class="sxs-lookup"><span data-stu-id="350fa-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="350fa-157">La clase HtmlHelper incluye propiedades estáticas de solo lectura para recuperar los nombres de la validación relacionados con CSS clases.</span><span class="sxs-lookup"><span data-stu-id="350fa-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="350fa-158">Estas propiedades estáticas se denominan ValidationInputCssClassName, ValidationFieldCssClassName y ValidationSummaryCssClassName.</span><span class="sxs-lookup"><span data-stu-id="350fa-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="350fa-159">Prebinding validación y la validación de Postbinding</span><span class="sxs-lookup"><span data-stu-id="350fa-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="350fa-160">Si se envía el formulario HTML para la creación de un producto y escriba un valor no válido para el campo de precio y ningún valor para el campo UnitsInStock, obtendrá los mensajes de validación que se muestra en la figura 4.</span><span class="sxs-lookup"><span data-stu-id="350fa-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="350fa-161">¿De dónde proceden estos mensajes de error de validación?</span><span class="sxs-lookup"><span data-stu-id="350fa-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="350fa-162">[![El cuadro de diálogo nuevo proyecto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="350fa-162">[![The New Project dialog box](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)</span></span>

<span data-ttu-id="350fa-163">**Figura 04**: errores de validación de Prebinding ([haga clic aquí para ver la imagen a tamaño completo](performing-simple-validation-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="350fa-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-vb/_static/image8.png))</span></span>


<span data-ttu-id="350fa-164">Existen realmente dos tipos de mensajes de error de validación - los generados antes de que los campos de formulario HTML están enlazados a una clase y las que generan después de los campos del formulario se enlazan a la clase.</span><span class="sxs-lookup"><span data-stu-id="350fa-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="350fa-165">En otras palabras, no hay errores de validación de prebinding y postbinding errores de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="350fa-166">La acción de Create() expuesta por el controlador de producto en la lista 1 acepta una instancia de la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="350fa-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="350fa-167">La firma del método Create tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="350fa-167">The signature of the Create method looks like this:</span></span>

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

<span data-ttu-id="350fa-168">Los valores de los campos de formulario HTML desde el formulario de creación se enlazan a la clase productToCreate por lo que se denomina un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="350fa-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="350fa-169">El enlazador de modelos predeterminado agrega un mensaje de error al estado modelo automáticamente cuando un campo de formulario no puede enlazar a una propiedad de formulario.</span><span class="sxs-lookup"><span data-stu-id="350fa-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="350fa-170">El enlazador de modelos predeterminado no puede enlazar la cadena "apple" para la propiedad de precio de la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="350fa-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="350fa-171">No se puede asignar una cadena a una propiedad decimal.</span><span class="sxs-lookup"><span data-stu-id="350fa-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="350fa-172">Por lo tanto, el enlazador de modelos agrega un error al estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="350fa-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="350fa-173">El enlazador de modelos predeterminado también no puede asignar el valor Nothing a una propiedad que no acepta el valor Nothing.</span><span class="sxs-lookup"><span data-stu-id="350fa-173">The default model binder also cannot assign the value Nothing to a property that does not accept the value Nothing.</span></span> <span data-ttu-id="350fa-174">En concreto, el enlazador de modelos no puede asignar el valor Nothing a la propiedad UnitsInStock.</span><span class="sxs-lookup"><span data-stu-id="350fa-174">In particular, the model binder cannot assign the value Nothing to the UnitsInStock property.</span></span> <span data-ttu-id="350fa-175">Una vez más, el enlazador de modelos desiste y agrega un mensaje de error al estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="350fa-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="350fa-176">Si desea personalizar la apariencia de estos prebinding mensajes de error, a continuación, debe crear cadenas de recursos para estos mensajes.</span><span class="sxs-lookup"><span data-stu-id="350fa-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="350fa-177">Resumen</span><span class="sxs-lookup"><span data-stu-id="350fa-177">Summary</span></span>

<span data-ttu-id="350fa-178">El objetivo de este tutorial era describir los mecanismos básicos de validación en el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="350fa-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="350fa-179">Aprendió a utilizar el estado del modelo y las aplicaciones auxiliares HTML de validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="350fa-180">También se explica la distinción entre prebinding y postbinding validación.</span><span class="sxs-lookup"><span data-stu-id="350fa-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="350fa-181">En otros tutoriales, analizaremos diversas estrategias para mover el código de validación de los controladores y en las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="350fa-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="350fa-182">[Anterior](displaying-a-table-of-database-data-vb.md)
[Siguiente](validating-with-the-idataerrorinfo-interface-vb.md)</span><span class="sxs-lookup"><span data-stu-id="350fa-182">[Previous](displaying-a-table-of-database-data-vb.md)
[Next](validating-with-the-idataerrorinfo-interface-vb.md)</span></span>
