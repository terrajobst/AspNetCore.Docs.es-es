---
title: Agregar la validación a una página de Razor de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar validación a una página de Razor de ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: d4cc0ab9de314c0c5a1a9016efd1e566ff1c47d2
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505783"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="ce068-103">Agregar la validación a una página de Razor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce068-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="ce068-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ce068-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce068-105">En esta sección se agrega lógica de validación al modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ce068-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="ce068-106">Las reglas de validación se aplican cada vez que un usuario crea o edita una película.</span><span class="sxs-lookup"><span data-stu-id="ce068-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="ce068-107">Validación</span><span class="sxs-lookup"><span data-stu-id="ce068-107">Validation</span></span>

<span data-ttu-id="ce068-108">Un principio clave de desarrollo de software se denomina [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself), por "**D**on't **R**epeat **Y**ourself" (Una vez y solo una).</span><span class="sxs-lookup"><span data-stu-id="ce068-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="ce068-109">Las páginas de Razor fomentan un tipo de desarrollo en el que la funcionalidad se especifica una vez y se refleja en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ce068-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="ce068-110">DRY puede ayudar a reducir la cantidad de código en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="ce068-110">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="ce068-111">DRY hace que el código sea menos propenso a errores y resulte más fácil de probar y mantener.</span><span class="sxs-lookup"><span data-stu-id="ce068-111">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="ce068-112">La compatibilidad de validación proporcionada por las páginas de Razor y Entity Framework es un buen ejemplo del principio DRY.</span><span class="sxs-lookup"><span data-stu-id="ce068-112">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="ce068-113">Las reglas de validación se especifican mediante declaración en un solo lugar (en la clase del modelo) y se aplican en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ce068-113">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="ce068-114">Adición de reglas de validación al modelo de película</span><span class="sxs-lookup"><span data-stu-id="ce068-114">Adding validation rules to the movie model</span></span>

<span data-ttu-id="ce068-115">Abra el archivo *Models/Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce068-115">Open the *Models/Movie.cs* file.</span></span> <span data-ttu-id="ce068-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) proporciona un conjunto integrado de atributos de validación que se aplican mediante declaración a una clase o propiedad.</span><span class="sxs-lookup"><span data-stu-id="ce068-116">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="ce068-117">DataAnnotations también contiene atributos de formato como `DataType` que ayudan a aplicar formato y no proporcionan validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="ce068-118">Actualice la clase `Movie` para aprovechar los atributos de validación `Required`, `StringLength`, `RegularExpression` y `Range`.</span><span class="sxs-lookup"><span data-stu-id="ce068-118">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce068-119">Los atributos de validación especifican el comportamiento que se aplica a las propiedades del modelo:</span><span class="sxs-lookup"><span data-stu-id="ce068-119">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="ce068-120">Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor.</span><span class="sxs-lookup"><span data-stu-id="ce068-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="ce068-121">Aun así, nada impide que el usuario escriba un espacio en blanco para cumplir la restricción de validación de un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="ce068-121">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="ce068-122">Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que no aceptan valores NULL (como `decimal`, `int`, `float` y `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `Required`.</span><span class="sxs-lookup"><span data-stu-id="ce068-122">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="ce068-123">El atributo `RegularExpression` limita los caracteres que el usuario puede escribir.</span><span class="sxs-lookup"><span data-stu-id="ce068-123">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="ce068-124">En el código anterior, `Genre` debe comenzar por una o varias letras en mayúscula y continuar con cero o más letras, comillas simples o dobles, caracteres de espacio en blanco o guiones.</span><span class="sxs-lookup"><span data-stu-id="ce068-124">In the preceding code, `Genre` must start with one or more capital letters and follow with zero or more letters, single or double quotes, whitespace characters, or dashes.</span></span> <span data-ttu-id="ce068-125">`Rating` debe comenzar por una o varias letras en mayúscula y continuar con cero o más letras, números, comillas simples o dobles, caracteres de espacio en blanco o guiones.</span><span class="sxs-lookup"><span data-stu-id="ce068-125">`Rating` must start with one or more capital letters and follow with zero or more letters, numbers, single or double quotes, whitespace characters, or dashes.</span></span>
* <span data-ttu-id="ce068-126">El atributo `Range` restringe un valor a un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="ce068-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="ce068-127">El atributo `StringLength` establece la longitud máxima de una cadena y, de forma opcional, la longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="ce068-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="ce068-128">El que ASP.NET Core aplique automáticamente las reglas de validación ayuda a que una aplicación sea más sólida.</span><span class="sxs-lookup"><span data-stu-id="ce068-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="ce068-129">La validación automática en modelos ayuda a proteger la aplicación, ya que no hay que acordarse de su aplicación cuando se agrega nuevo código.</span><span class="sxs-lookup"><span data-stu-id="ce068-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="ce068-130">Interfaz de usuario de error de validación en páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="ce068-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="ce068-131">Ejecute la aplicación y vaya a Pages/Movies.</span><span class="sxs-lookup"><span data-stu-id="ce068-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="ce068-132">Seleccione el vínculo **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="ce068-132">Select the **Create New** link.</span></span> <span data-ttu-id="ce068-133">Rellene el formulario con algunos valores no válidos.</span><span class="sxs-lookup"><span data-stu-id="ce068-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="ce068-134">Cuando la validación de cliente de jQuery detecta el error, muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="ce068-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Formulario de vista de película con varios errores de validación de cliente de jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

<span data-ttu-id="ce068-136">Observe cómo el formulario presenta automáticamente un mensaje de error de validación en cada campo que contiene un valor no válido.</span><span class="sxs-lookup"><span data-stu-id="ce068-136">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="ce068-137">Los errores se aplican al cliente (con JavaScript y jQuery) y al servidor (cuando un usuario tiene JavaScript deshabilitado).</span><span class="sxs-lookup"><span data-stu-id="ce068-137">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="ce068-138">Una ventaja importante es que **no** se han necesitado cambios de código en las páginas de creación o edición.</span><span class="sxs-lookup"><span data-stu-id="ce068-138">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="ce068-139">Una vez que DataAnnotations se ha aplicado al modelo, la interfaz de usuario de validación se ha habilitado.</span><span class="sxs-lookup"><span data-stu-id="ce068-139">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="ce068-140">Las páginas de Razor creadas en este tutorial han obtenido automáticamente las reglas de validación (mediante atributos de validación en las propiedades de la clase del modelo `Movie`).</span><span class="sxs-lookup"><span data-stu-id="ce068-140">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="ce068-141">Al probar la validación en la página de edición, se aplica la misma validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-141">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="ce068-142">Los datos del formulario no se publicarán en el servidor hasta que dejen de producirse errores de validación de cliente.</span><span class="sxs-lookup"><span data-stu-id="ce068-142">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="ce068-143">Compruebe que los datos del formulario no se publican mediante uno o varios de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ce068-143">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="ce068-144">Coloque un punto de interrupción en el método `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce068-144">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="ce068-145">Envíe el formulario (seleccione **Crear** o **Guardar**).</span><span class="sxs-lookup"><span data-stu-id="ce068-145">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="ce068-146">El punto de interrupción nunca se alcanza.</span><span class="sxs-lookup"><span data-stu-id="ce068-146">The break point is never hit.</span></span>
* <span data-ttu-id="ce068-147">Use la [herramienta Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="ce068-147">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="ce068-148">Use las herramientas de desarrollo del explorador para supervisar el tráfico de red.</span><span class="sxs-lookup"><span data-stu-id="ce068-148">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="ce068-149">Validación de servidor</span><span class="sxs-lookup"><span data-stu-id="ce068-149">Server-side validation</span></span>

<span data-ttu-id="ce068-150">Cuando JavaScript está deshabilitado en el explorador, si se envía el formulario con errores, se publica en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ce068-150">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="ce068-151">Validación de servidor de prueba opcional:</span><span class="sxs-lookup"><span data-stu-id="ce068-151">Optional, test server-side validation:</span></span>

* <span data-ttu-id="ce068-152">Deshabilite JavaScript en el explorador.</span><span class="sxs-lookup"><span data-stu-id="ce068-152">Disable JavaScript in the browser.</span></span> <span data-ttu-id="ce068-153">Puede hacerlo con las herramientas para desarrolladores del explorador.</span><span class="sxs-lookup"><span data-stu-id="ce068-153">You can do this using your browser's developer tools.</span></span> <span data-ttu-id="ce068-154">Si no se puede deshabilitar JavaScript en el explorador, pruebe con otro explorador.</span><span class="sxs-lookup"><span data-stu-id="ce068-154">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="ce068-155">Establezca un punto de interrupción en el método `OnPostAsync` de la página de creación o edición.</span><span class="sxs-lookup"><span data-stu-id="ce068-155">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="ce068-156">Envíe un formulario con errores de validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-156">Submit a form with validation errors.</span></span>
* <span data-ttu-id="ce068-157">Compruebe que el estado del modelo no es válido:</span><span class="sxs-lookup"><span data-stu-id="ce068-157">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="ce068-158">El código siguiente muestra una parte de la página *Create.cshtml* a la que se ha aplicado scaffolding anteriormente en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="ce068-158">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="ce068-159">Las páginas de creación y edición la usan para mostrar el formulario inicial y para volver a mostrar el formulario en caso de error.</span><span class="sxs-lookup"><span data-stu-id="ce068-159">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="ce068-160">El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ce068-160">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="ce068-161">El [asistente de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) muestra errores de validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-161">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="ce068-162">Para más información, vea [Validación](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="ce068-162">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="ce068-163">Las páginas de creación y edición no tienen ninguna regla de validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-163">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="ce068-164">Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ce068-164">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="ce068-165">Estas reglas de validación se aplican automáticamente a las páginas de Razor que editan el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ce068-165">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="ce068-166">Cuando es necesario modificar la lógica de validación, se hace únicamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="ce068-166">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="ce068-167">La validación se aplica de forma coherente en toda la aplicación (la lógica de validación se define en un solo lugar).</span><span class="sxs-lookup"><span data-stu-id="ce068-167">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="ce068-168">La validación en un solo lugar ayuda a mantener limpio el código y facilita su mantenimiento y actualización.</span><span class="sxs-lookup"><span data-stu-id="ce068-168">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="ce068-169">Uso de atributos DataType</span><span class="sxs-lookup"><span data-stu-id="ce068-169">Using DataType Attributes</span></span>

<span data-ttu-id="ce068-170">Examine la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ce068-170">Examine the `Movie` class.</span></span> <span data-ttu-id="ce068-171">El espacio de nombres `System.ComponentModel.DataAnnotations` proporciona atributos de formato además del conjunto integrado de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-171">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="ce068-172">El atributo `DataType` se aplica a las propiedades `ReleaseDate` y `Price`.</span><span class="sxs-lookup"><span data-stu-id="ce068-172">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="ce068-173">Los atributos `DataType` solo proporcionan sugerencias para que el motor de vista aplique formato a los datos (y ofrece atributos como `<a>` para las direcciones URL y `<a href="mailto:EmailAddress.com">` para el correo electrónico).</span><span class="sxs-lookup"><span data-stu-id="ce068-173">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="ce068-174">Use el atributo `RegularExpression` para validar el formato de los datos.</span><span class="sxs-lookup"><span data-stu-id="ce068-174">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="ce068-175">El atributo `DataType` se usa para especificar un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ce068-175">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="ce068-176">Los atributos `DataType` no son atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-176">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="ce068-177">En la aplicación de ejemplo solo se muestra la fecha, sin hora.</span><span class="sxs-lookup"><span data-stu-id="ce068-177">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="ce068-178">La enumeración `DataType` proporciona muchos tipos de datos, como Date, Time, PhoneNumber, Currency, EmailAddress, etc.</span><span class="sxs-lookup"><span data-stu-id="ce068-178">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="ce068-179">El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="ce068-179">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="ce068-180">Por ejemplo, se puede crear un vínculo `mailto:` para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="ce068-180">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="ce068-181">Se puede proporcionar un selector de fecha para `DataType.Date` en exploradores compatibles con HTML5.</span><span class="sxs-lookup"><span data-stu-id="ce068-181">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="ce068-182">Los atributos `DataType` emiten atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="ce068-182">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="ce068-183">Los atributos `DataType` **no** proporcionan ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="ce068-183">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="ce068-184">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="ce068-184">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="ce068-185">De manera predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el elemento `CultureInfo` del servidor.</span><span class="sxs-lookup"><span data-stu-id="ce068-185">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ce068-186">La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` es necesaria para que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ce068-186">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="ce068-187">Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="ce068-187">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

::: moniker-end

<span data-ttu-id="ce068-188">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="ce068-188">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="ce068-189">El valor `ApplyFormatInEditMode` especifica que el formato se debe aplicar cuando el valor se muestra para su edición.</span><span class="sxs-lookup"><span data-stu-id="ce068-189">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="ce068-190">Es posible que no quiera ese comportamiento para algunos campos.</span><span class="sxs-lookup"><span data-stu-id="ce068-190">You might not want that behavior for some fields.</span></span> <span data-ttu-id="ce068-191">Por ejemplo, en los valores de moneda, probablemente no quiera el símbolo de moneda en la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="ce068-191">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="ce068-192">El atributo `DisplayFormat` puede usarse por sí mismo, pero normalmente es buena idea usar el atributo `DataType`.</span><span class="sxs-lookup"><span data-stu-id="ce068-192">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="ce068-193">El atributo `DataType` transmite la semántica de los datos en contraposición a cómo se representan en una pantalla y ofrece las siguientes ventajas que no proporciona DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="ce068-193">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="ce068-194">El explorador puede habilitar características de HTML5 (por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado según la configuración regional, vínculos de correo electrónico, etc.).</span><span class="sxs-lookup"><span data-stu-id="ce068-194">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="ce068-195">De forma predeterminada, el explorador presenta los datos con el formato correcto en función de la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="ce068-195">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="ce068-196">El atributo `DataType` puede habilitar el marco de trabajo de ASP.NET Core para elegir la plantilla de campo correcta a fin de presentar los datos.</span><span class="sxs-lookup"><span data-stu-id="ce068-196">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="ce068-197">`DisplayFormat`, si se emplea por sí mismo, usa la plantilla de cadena.</span><span class="sxs-lookup"><span data-stu-id="ce068-197">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="ce068-198">Nota: La validación de jQuery no funciona con el atributo `Range` ni `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="ce068-198">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="ce068-199">Por ejemplo, el código siguiente siempre muestra un error de validación de cliente, incluso cuando la fecha está en el intervalo especificado:</span><span class="sxs-lookup"><span data-stu-id="ce068-199">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="ce068-200">Por lo general no se recomienda compilar fechas fijas en los modelos, así que se desaconseja el empleo del atributo `Range` y `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="ce068-200">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="ce068-201">El código siguiente muestra la combinación de atributos en una línea:</span><span class="sxs-lookup"><span data-stu-id="ce068-201">The following code shows combining attributes on one line:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ce068-202">En [Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) (Introducción a Razor Pages y EF Core) se muestran operaciones avanzadas de EF Core con Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ce068-202">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="publish-to-azure"></a><span data-ttu-id="ce068-203">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="ce068-203">Publish to Azure</span></span>

<span data-ttu-id="ce068-204">Para obtener información sobre la implementación en Azure, vea [Tutorial: Creación de una aplicación ASP.NET en Azure con SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span><span class="sxs-lookup"><span data-stu-id="ce068-204">For information on deploying to Azure, See [Tutorial: Build an ASP.NET app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase).</span></span> <span data-ttu-id="ce068-205">Estas instrucciones son para una aplicación ASP.NET, no para una aplicación ASP.NET Core, pero los pasos son los mismos.</span><span class="sxs-lookup"><span data-stu-id="ce068-205">These instructions are for an ASP.NET app, not an ASP.NET Core app, but the steps are the same.</span></span>

<span data-ttu-id="ce068-206">Gracias por seguir esta introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="ce068-206">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="ce068-207">Le agradeceremos cualquier comentario.</span><span class="sxs-lookup"><span data-stu-id="ce068-207">We appreciate feedback.</span></span> <span data-ttu-id="ce068-208">[Introducción a MVC con Razor Pages y EF Core](xref:data/ef-rp/intro) es un excelente artículo de seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ce068-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce068-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ce068-209">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce068-210">Anterior: Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="ce068-210">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
