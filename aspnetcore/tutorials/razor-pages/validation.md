---
title: Agregar la validación a una página de Razor de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar validación a una página de Razor de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/validation
ms.openlocfilehash: c2397a535fa2c128f18d65323d0f4920af914205
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334220"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="b5c9b-103">Agregar la validación a una página de Razor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5c9b-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="b5c9b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5c9b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5c9b-105">En esta sección se agrega lógica de validación al modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="b5c9b-106">Las reglas de validación se aplican cada vez que un usuario crea o edita una película.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="b5c9b-107">Validación</span><span class="sxs-lookup"><span data-stu-id="b5c9b-107">Validation</span></span>

<span data-ttu-id="b5c9b-108">Un principio clave de desarrollo de software se denomina [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself), por "**D**on't **R**epeat **Y**ourself" (Una vez y solo una).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="b5c9b-109">Las páginas de Razor fomentan un tipo de desarrollo en el que la funcionalidad se especifica una vez y se refleja en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="b5c9b-110">DRY puede ayudar a:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-110">DRY can help:</span></span>

* <span data-ttu-id="b5c9b-111">Reducir la cantidad de código en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="b5c9b-112">Hacer que el código sea menos propenso a errores y resulte más fácil de probar y mantener.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="b5c9b-113">La compatibilidad de validación proporcionada por las páginas de Razor y Entity Framework es un buen ejemplo del principio DRY.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="b5c9b-114">Las reglas de validación se especifican mediante declaración en un solo lugar (en la clase del modelo) y se aplican en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

## <a name="add-validation-rules-to-the-movie-model"></a><span data-ttu-id="b5c9b-115">Adición de reglas de validación al modelo de película</span><span class="sxs-lookup"><span data-stu-id="b5c9b-115">Add validation rules to the movie model</span></span>

<span data-ttu-id="b5c9b-116">El espacio de nombres DataAnnotations proporciona un conjunto de atributos de validación integrados que se aplican mediante declaración a una clase o propiedad.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-116">The DataAnnotations namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="b5c9b-117">DataAnnotations también contiene atributos de formato como `DataType` que ayudan a aplicar formato y no proporcionan ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="b5c9b-118">Actualice la clase `Movie` para aprovechar los atributos de validación integrados `Required`, `StringLength`, `RegularExpression` y `Range`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-118">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="b5c9b-119">Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-119">The validation attributes specify behavior that you want to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="b5c9b-120">Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor, pero nada impide al usuario escribir espacios en blanco para satisfacer esta validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="b5c9b-121">El atributo `RegularExpression` se usa para limitar los caracteres que se pueden escribir.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-121">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="b5c9b-122">En el código anterior, "Género":</span><span class="sxs-lookup"><span data-stu-id="b5c9b-122">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="b5c9b-123">Solo debe usar letras.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-123">Must only use letters.</span></span>
  * <span data-ttu-id="b5c9b-124">La primera letra debe estar en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-124">The first letter is required to be uppercase.</span></span> <span data-ttu-id="b5c9b-125">No se permiten espacios en blanco, números ni caracteres especiales.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-125">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="b5c9b-126">La "Clasificación" de `RegularExpression`:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-126">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="b5c9b-127">Requiere que el primer carácter sea una letra mayúscula.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-127">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="b5c9b-128">Permite caracteres especiales y números en los espacios posteriores.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-128">Allows special characters and numbers in  subsequent spaces.</span></span> <span data-ttu-id="b5c9b-129">"PG-13" es válido para una "Clasificación", pero se produce un error en un "Género".</span><span class="sxs-lookup"><span data-stu-id="b5c9b-129">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="b5c9b-130">El atributo `Range` restringe un valor a un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-130">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="b5c9b-131">El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-131">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="b5c9b-132">Los tipos de valor (como `decimal`, `int`, `float`, `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-132">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="b5c9b-133">El que ASP.NET Core aplique automáticamente las reglas de validación ayuda a que su aplicación sea más sólida.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-133">Having validation rules automatically enforced by ASP.NET Core helps make your app more robust.</span></span> <span data-ttu-id="b5c9b-134">También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-134">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="b5c9b-135">Interfaz de usuario de error de validación en páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="b5c9b-135">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="b5c9b-136">Ejecute la aplicación y vaya a Pages/Movies.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-136">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="b5c9b-137">Seleccione el vínculo **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-137">Select the **Create New** link.</span></span> <span data-ttu-id="b5c9b-138">Rellene el formulario con algunos valores no válidos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-138">Complete the form with some invalid values.</span></span> <span data-ttu-id="b5c9b-139">Cuando la validación de cliente de jQuery detecta el error, muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-139">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Formulario de vista de película con varios errores de validación de cliente de jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

<span data-ttu-id="b5c9b-141">Observe cómo el formulario presenta automáticamente un mensaje de error de validación en cada campo que contiene un valor no válido.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-141">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="b5c9b-142">Los errores se aplican al cliente (con JavaScript y jQuery) y al servidor (cuando un usuario tiene JavaScript deshabilitado).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-142">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="b5c9b-143">Una ventaja importante es que **no** se han necesitado cambios de código en las páginas de creación o edición.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-143">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="b5c9b-144">Una vez que DataAnnotations se ha aplicado al modelo, la interfaz de usuario de validación se ha habilitado.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-144">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="b5c9b-145">Las páginas de Razor creadas en este tutorial han obtenido automáticamente las reglas de validación (mediante atributos de validación en las propiedades de la clase del modelo `Movie`).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-145">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="b5c9b-146">Al probar la validación en la página de edición, se aplica la misma validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-146">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="b5c9b-147">Los datos del formulario no se publicarán en el servidor hasta que dejen de producirse errores de validación de cliente.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-147">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="b5c9b-148">Compruebe que los datos del formulario no se publican mediante uno o varios de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-148">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="b5c9b-149">Coloque un punto de interrupción en el método `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-149">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="b5c9b-150">Envíe el formulario (seleccione **Crear** o **Guardar**).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-150">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="b5c9b-151">El punto de interrupción nunca se alcanza.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-151">The break point is never hit.</span></span>
* <span data-ttu-id="b5c9b-152">Use la [herramienta Fiddler](https://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-152">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="b5c9b-153">Use las herramientas de desarrollo del explorador para supervisar el tráfico de red.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-153">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="b5c9b-154">Validación de servidor</span><span class="sxs-lookup"><span data-stu-id="b5c9b-154">Server-side validation</span></span>

<span data-ttu-id="b5c9b-155">Cuando JavaScript está deshabilitado en el explorador, si se envía el formulario con errores, se publica en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-155">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="b5c9b-156">Validación de servidor de prueba opcional:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-156">Optional, test server-side validation:</span></span>

* <span data-ttu-id="b5c9b-157">Deshabilite JavaScript en el explorador.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-157">Disable JavaScript in the browser.</span></span> <span data-ttu-id="b5c9b-158">Puede deshabilitar JavaScript con las herramientas para el desarrollador del explorador.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-158">You can disable JavaScript using browser's developer tools.</span></span> <span data-ttu-id="b5c9b-159">Si no se puede deshabilitar JavaScript en el explorador, pruebe con otro explorador.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-159">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="b5c9b-160">Establezca un punto de interrupción en el método `OnPostAsync` de la página de creación o edición.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-160">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="b5c9b-161">Envíe un formulario con datos no válidos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-161">Submit a form with invalid data.</span></span>
* <span data-ttu-id="b5c9b-162">Compruebe que el estado del modelo no es válido:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-162">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="b5c9b-163">El código siguiente muestra una parte de la página *Create.cshtml* a la que se aplicó scaffolding anteriormente en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-163">The following code shows a portion of the *Create.cshtml* page scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="b5c9b-164">Las páginas de creación y edición la usan para mostrar el formulario inicial y para volver a mostrar el formulario en caso de error.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-164">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="b5c9b-165">El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el cliente.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-165">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="b5c9b-166">El [asistente de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) muestra errores de validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-166">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="b5c9b-167">Para más información, vea [Validación](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-167">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="b5c9b-168">Las páginas de creación y edición no tienen ninguna regla de validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-168">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="b5c9b-169">Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-169">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="b5c9b-170">Estas reglas de validación se aplican automáticamente a las páginas de Razor que editan el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-170">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="b5c9b-171">Cuando es necesario modificar la lógica de validación, se hace únicamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-171">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="b5c9b-172">La validación se aplica de forma coherente en toda la aplicación (la lógica de validación se define en un solo lugar).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-172">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="b5c9b-173">La validación en un solo lugar ayuda a mantener limpio el código y facilita su mantenimiento y actualización.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-173">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="b5c9b-174">Uso de atributos DataType</span><span class="sxs-lookup"><span data-stu-id="b5c9b-174">Using DataType Attributes</span></span>

<span data-ttu-id="b5c9b-175">Examine la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-175">Examine the `Movie` class.</span></span> <span data-ttu-id="b5c9b-176">El espacio de nombres `System.ComponentModel.DataAnnotations` proporciona atributos de formato además del conjunto integrado de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-176">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="b5c9b-177">El atributo `DataType` se aplica a las propiedades `ReleaseDate` y `Price`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-177">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="b5c9b-178">Los atributos `DataType` solo proporcionan sugerencias para que el motor de vista aplique formato a los datos (y ofrece atributos como `<a>` para las direcciones URL y `<a href="mailto:EmailAddress.com">` para el correo electrónico).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-178">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="b5c9b-179">Use el atributo `RegularExpression` para validar el formato de los datos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-179">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="b5c9b-180">El atributo `DataType` se usa para especificar un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-180">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="b5c9b-181">Los atributos `DataType` no son atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-181">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="b5c9b-182">En la aplicación de ejemplo solo se muestra la fecha, sin hora.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-182">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="b5c9b-183">La enumeración `DataType` proporciona muchos tipos de datos, como Date, Time, PhoneNumber, Currency, EmailAddress, etc.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-183">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="b5c9b-184">El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-184">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="b5c9b-185">Por ejemplo, se puede crear un vínculo `mailto:` para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-185">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="b5c9b-186">Se puede proporcionar un selector de fecha para `DataType.Date` en exploradores compatibles con HTML5.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-186">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="b5c9b-187">Los atributos `DataType` emiten atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-187">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="b5c9b-188">Los atributos `DataType` **no** proporcionan ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-188">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="b5c9b-189">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-189">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="b5c9b-190">De manera predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el elemento `CultureInfo` del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-190">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="b5c9b-191">La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` es necesaria para que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-191">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="b5c9b-192">Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-192">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="b5c9b-193">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-193">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="b5c9b-194">El valor `ApplyFormatInEditMode` especifica que el formato se debe aplicar cuando el valor se muestra para su edición.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-194">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="b5c9b-195">Es posible que no quiera ese comportamiento para algunos campos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-195">You might not want that behavior for some fields.</span></span> <span data-ttu-id="b5c9b-196">Por ejemplo, en los valores de moneda, probablemente no quiera el símbolo de moneda en la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-196">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="b5c9b-197">El atributo `DisplayFormat` puede usarse por sí mismo, pero normalmente es buena idea usar el atributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-197">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="b5c9b-198">El atributo `DataType` transmite la semántica de los datos en contraposición a cómo se representan en una pantalla y ofrece las siguientes ventajas que no proporciona DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-198">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="b5c9b-199">El explorador puede habilitar características de HTML5 (por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado según la configuración regional, vínculos de correo electrónico, etc.).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-199">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="b5c9b-200">De forma predeterminada, el explorador presenta los datos con el formato correcto en función de la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-200">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="b5c9b-201">El atributo `DataType` puede habilitar el marco de trabajo de ASP.NET Core para elegir la plantilla de campo correcta a fin de presentar los datos.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-201">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="b5c9b-202">`DisplayFormat`, si se emplea por sí mismo, usa la plantilla de cadena.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-202">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="b5c9b-203">Nota: La validación de jQuery no funciona con el atributo `Range` ni `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-203">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="b5c9b-204">Por ejemplo, el código siguiente siempre muestra un error de validación de cliente, incluso cuando la fecha está en el intervalo especificado:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-204">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="b5c9b-205">Por lo general no se recomienda compilar fechas fijas en los modelos, así que se desaconseja el empleo del atributo `Range` y `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-205">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="b5c9b-206">El código siguiente muestra la combinación de atributos en una línea:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-206">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="b5c9b-207">En [Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) (Introducción a Razor Pages y EF Core) se muestran operaciones avanzadas de EF Core con Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-207">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="b5c9b-208">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="b5c9b-208">Apply migrations</span></span>

<span data-ttu-id="b5c9b-209">Las DataAnnotations aplicadas a la clase cambian el esquema.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-209">The DataAnnotations applied to the class change the schema.</span></span> <span data-ttu-id="b5c9b-210">Por ejemplo, las DataAnnotations aplicadas al campo `Title`:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-210">For example, the DataAnnotations applied to the `Title` field:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* <span data-ttu-id="b5c9b-211">Limita los caracteres a 60.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-211">Limits the characters to 60.</span></span>
* <span data-ttu-id="b5c9b-212">No permite un valor `null`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-212">Doesn't allow a `null` value.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b5c9b-213">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5c9b-213">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b5c9b-214">La tabla `Movie` tiene actualmente el esquema siguiente:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-214">The `Movie` table currently has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

<span data-ttu-id="b5c9b-215">Los cambios en el esquema anterior no hacen que EF genere una excepción.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-215">The preceding schema changes don't cause EF to throw an exception.</span></span> <span data-ttu-id="b5c9b-216">Sin embargo, cree una migración para que el esquema sea coherente con el modelo.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-216">However, create a migration so the schema is consistent with the model.</span></span>

<span data-ttu-id="b5c9b-217">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-217">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="b5c9b-218">En PCM, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-218">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

<span data-ttu-id="b5c9b-219">`Update-Database` ejecuta los métodos `Up` de la clase `New_DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-219">`Update-Database` runs the `Up` methods of the `New_DataAnnotations` class.</span></span> <span data-ttu-id="b5c9b-220">Examine el método `Up`:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-220">Examine the `Up` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

<span data-ttu-id="b5c9b-221">La tabla `Movie` actualizada tiene el esquema siguiente:</span><span class="sxs-lookup"><span data-stu-id="b5c9b-221">The updated `Movie` table has the following schema:</span></span>

``` sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b5c9b-222">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="b5c9b-222">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b5c9b-223">No se requieren migraciones para SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-223">Migrations are not required for SQLite.</span></span>

---

### <a name="publish-to-azure"></a><span data-ttu-id="b5c9b-224">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="b5c9b-224">Publish to Azure</span></span>

<span data-ttu-id="b5c9b-225">Para obtener información sobre la implementación en Azure, consulte [Tutorial: Compilación de una aplicación ASP.NET Core en Azure con SQL Database](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span><span class="sxs-lookup"><span data-stu-id="b5c9b-225">For information on deploying to Azure, see [Tutorial: Build an ASP.NET Core app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span></span>

<span data-ttu-id="b5c9b-226">Gracias por seguir esta introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-226">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="b5c9b-227">[Introducción a MVC con Razor Pages y EF Core](xref:data/ef-rp/intro) es un excelente artículo de seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="b5c9b-227">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5c9b-228">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b5c9b-228">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="b5c9b-229">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="b5c9b-229">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> <span data-ttu-id="b5c9b-230">[Anterior: ](xref:tutorials/razor-pages/new-field)Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="b5c9b-230">[Previous: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
