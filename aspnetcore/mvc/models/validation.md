---
title: Validación de modelos en ASP.NET Core MVC
author: rick-anderson
description: Obtenga información sobre la validación de modelos en ASP.NET Core MVC y Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
uid: mvc/models/validation
ms.openlocfilehash: 19f71799e958e2761832c91cec6762a6d391d2b5
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317425"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="49507-103">Validación de modelos en ASP.NET Core MVC y Razor Pages</span><span class="sxs-lookup"><span data-stu-id="49507-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="49507-104">De [Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="49507-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="49507-105">En este artículo se explica cómo validar la entrada del usuario en una aplicación ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49507-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="49507-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="49507-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="49507-107">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="49507-107">Model state</span></span>

<span data-ttu-id="49507-108">El estado del modelo representa los errores que proceden de dos subsistemas: el enlace de modelos y la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="49507-109">Los errores que se originan del [enlace de modelos](model-binding.md) suelen ser errores de conversión de datos.</span><span class="sxs-lookup"><span data-stu-id="49507-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="49507-110">Por ejemplo, se escribe una "x" en un campo numérico entero.</span><span class="sxs-lookup"><span data-stu-id="49507-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="49507-111">La validación del modelo se produce después del enlace de modelos y notifica los errores en los que los datos no cumplen las reglas de negocio.</span><span class="sxs-lookup"><span data-stu-id="49507-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="49507-112">Por ejemplo, se especifica un 0 en un campo que espera una clasificación entre 1 y 5.</span><span class="sxs-lookup"><span data-stu-id="49507-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="49507-113">Tanto el enlace como la validación de modelos se producen antes de la ejecución de una acción de controlador o un método de controlador de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49507-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="49507-114">En el caso de las aplicaciones web, la aplicación es responsable de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="49507-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="49507-115">Normalmente, las aplicaciones web vuelven a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="49507-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="49507-116">Los controladores de Web API no tienen que comprobar `ModelState.IsValid` si tienen el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="49507-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="49507-117">En ese caso, se devuelve una respuesta HTTP 400 automática que contiene los detalles del error cuando el estado del modelo no es válido.</span><span class="sxs-lookup"><span data-stu-id="49507-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="49507-118">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="49507-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="49507-119">Nueva ejecución de la validación</span><span class="sxs-lookup"><span data-stu-id="49507-119">Rerun validation</span></span>

<span data-ttu-id="49507-120">La validación es automática, pero tal vez le interese repetirla manualmente.</span><span class="sxs-lookup"><span data-stu-id="49507-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="49507-121">Por ejemplo, tal vez haya calculado el valor de una propiedad y quiera volver a ejecutar la validación después de establecer la propiedad en el valor calculado.</span><span class="sxs-lookup"><span data-stu-id="49507-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="49507-122">Para volver a ejecutar la validación, llame al método `TryValidateModel`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="49507-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="49507-123">Atributos de validación</span><span class="sxs-lookup"><span data-stu-id="49507-123">Validation attributes</span></span>

<span data-ttu-id="49507-124">Los atributos de validación permiten especificar reglas de validación para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="49507-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="49507-125">En el ejemplo siguiente de la aplicación de ejemplo se muestra una clase de modelo anotada con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="49507-126">El atributo `[ClassicMovie]` es un atributo de validación personalizado y los demás están integrados.</span><span class="sxs-lookup"><span data-stu-id="49507-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="49507-127">`[ClassicMovieWithClientValidator]` no se muestra.</span><span class="sxs-lookup"><span data-stu-id="49507-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="49507-128">`[ClassicMovieWithClientValidator]` muestra una manera alternativa de implementar un atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="49507-129">Atributos integrados</span><span class="sxs-lookup"><span data-stu-id="49507-129">Built-in attributes</span></span>

<span data-ttu-id="49507-130">Estos son algunos de los atributos de validación integrados:</span><span class="sxs-lookup"><span data-stu-id="49507-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="49507-131">`[CreditCard]`: valida que la propiedad tenga formato de tarjeta de crédito.</span><span class="sxs-lookup"><span data-stu-id="49507-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="49507-132">`[Compare]`: valida que dos propiedades de un modelo coincidan.</span><span class="sxs-lookup"><span data-stu-id="49507-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="49507-133">`[EmailAddress]`: valida que la propiedad tenga formato de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49507-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="49507-134">`[Phone]`: valida que la propiedad tenga formato de número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="49507-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="49507-135">`[Range]`: valida que el valor de propiedad se encuentre dentro de un intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="49507-136">`[RegularExpression]`: valida que el valor de propiedad coincida con una expresión regular especificada.</span><span class="sxs-lookup"><span data-stu-id="49507-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="49507-137">`[Required]`: valida que el campo no sea NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="49507-138">Vea [Atributo [Required]](#required-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-138">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="49507-139">`[StringLength]`: valida que un valor de propiedad de cadena no supere un límite de longitud especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="49507-140">`[Url]`: valida que la propiedad tenga un formato de URL.</span><span class="sxs-lookup"><span data-stu-id="49507-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="49507-141">`[Remote]`: valida la entrada en el cliente mediante una llamada a un método de acción en el servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="49507-142">Vea [Atributo [Remote]](#remote-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-142">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="49507-143">En el espacio de nombres [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) encontrará una lista completa de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="49507-144">Mensajes de error</span><span class="sxs-lookup"><span data-stu-id="49507-144">Error messages</span></span>

<span data-ttu-id="49507-145">Los atributos de validación permiten especificar el mensaje de error que se mostrará para una entrada no válida.</span><span class="sxs-lookup"><span data-stu-id="49507-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="49507-146">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="49507-147">Internamente, los atributos llaman a `String.Format` con un marcador de posición para el nombre de campo y, en ocasiones, marcadores de posición adicionales.</span><span class="sxs-lookup"><span data-stu-id="49507-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="49507-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="49507-149">Cuando se aplica a una propiedad `Name`, el mensaje de error creado por el código anterior sería "La longitud del nombre debe estar entre 6 y 8".</span><span class="sxs-lookup"><span data-stu-id="49507-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="49507-150">Para averiguar qué parámetros se pasan a `String.Format` para el mensaje de error de un atributo determinado, vea el [código fuente de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="49507-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="49507-151">Atributo [Required]</span><span class="sxs-lookup"><span data-stu-id="49507-151">[Required] attribute</span></span>

<span data-ttu-id="49507-152">De forma predeterminada, el sistema de validación trata las propiedades o los parámetros que no aceptan valores NULL como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-152">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="49507-153">Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) como `decimal` y `int` no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="49507-154">Validación de [Required] en el servidor</span><span class="sxs-lookup"><span data-stu-id="49507-154">[Required] validation on the server</span></span>

<span data-ttu-id="49507-155">En el servidor, si la propiedad es NULL, se considera que falta un valor requerido.</span><span class="sxs-lookup"><span data-stu-id="49507-155">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="49507-156">Un campo que no acepta valores NULL siempre es válido y el mensaje de error del atributo `[Required]` no se muestra nunca.</span><span class="sxs-lookup"><span data-stu-id="49507-156">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="49507-157">Aun así, el enlace de modelos para una propiedad que no acepta valores NULL podría fallar, lo que genera un mensaje de error como `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="49507-157">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="49507-158">Para especificar un mensaje de error personalizado para la validación del lado servidor de tipos que no aceptan valores NULL, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="49507-158">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="49507-159">Haga que el campo acepte valores NULL (por ejemplo, `decimal?` en lugar de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="49507-159">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="49507-160">Los tipos de valor [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) se tratan como tipos estándar que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-160">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="49507-161">Especifique el mensaje de error predeterminado que el enlace de modelos va a usar, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-161">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="49507-162">Para obtener más información sobre los errores de enlace de modelos para los que se pueden establecer mensajes predeterminados, vea <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="49507-162">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="49507-163">Validación de [Required] en el cliente</span><span class="sxs-lookup"><span data-stu-id="49507-163">[Required] validation on the client</span></span>

<span data-ttu-id="49507-164">Las cadenas y los tipos que no aceptan valores NULL se tratan de forma diferente en el cliente, en comparación con el servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-164">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="49507-165">En el cliente:</span><span class="sxs-lookup"><span data-stu-id="49507-165">On the client:</span></span>

* <span data-ttu-id="49507-166">Un valor se considera presente solo si se especifica una entrada para él.</span><span class="sxs-lookup"><span data-stu-id="49507-166">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="49507-167">Por lo tanto, la validación del lado cliente controla los tipos que no aceptan valores NULL del mismo modo que los tipos que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-167">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="49507-168">El método [required](https://jqueryvalidation.org/required-method/) de jQuery Validate considera que un espacio en blanco en un campo de cadena es una entrada válida.</span><span class="sxs-lookup"><span data-stu-id="49507-168">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="49507-169">La validación del lado servidor considera que un campo de cadena necesario no es válido si solo se especifica un espacio en blanco.</span><span class="sxs-lookup"><span data-stu-id="49507-169">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="49507-170">Como se indicó anteriormente, los tipos que no aceptan valores NULL se tratan como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-170">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="49507-171">Esto significa que se obtiene la validación del lado cliente incluso si no se aplica el atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-171">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="49507-172">Si no usa el atributo, aparecerá un mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49507-172">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="49507-173">Para especificar un mensaje de error personalizado, use el atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-173">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="49507-174">Atributo [Remote]</span><span class="sxs-lookup"><span data-stu-id="49507-174">[Remote] attribute</span></span>

<span data-ttu-id="49507-175">El atributo `[Remote]` implementa la validación del lado cliente que requiere llamar a un método en el servidor para determinar si la entrada del campo es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-175">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="49507-176">Por ejemplo, la aplicación podría tener que comprobar si un nombre de usuario ya está en uso.</span><span class="sxs-lookup"><span data-stu-id="49507-176">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="49507-177">Para implementar la validación remota:</span><span class="sxs-lookup"><span data-stu-id="49507-177">To implement remote validation:</span></span>

1. <span data-ttu-id="49507-178">Cree un método de acción para que lo llame JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49507-178">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="49507-179">El método [remote](https://jqueryvalidation.org/remote-method/) de jQuery Validate espera una respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="49507-179">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="49507-180">`true` significa que los datos de entrada son válidos.</span><span class="sxs-lookup"><span data-stu-id="49507-180">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="49507-181">`false`, `undefined` o `null` significan que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-181">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="49507-182">Muestre el mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49507-182">Display the default error message.</span></span>
   * <span data-ttu-id="49507-183">Cualquier otra cadena significa que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-183">Any other string means the input is invalid.</span></span> <span data-ttu-id="49507-184">Muestre la cadena como un mensaje de error personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-184">Display the string as a custom error message.</span></span>

   <span data-ttu-id="49507-185">A continuación encontrará un ejemplo de un método de acción que devuelve un mensaje de error personalizado:</span><span class="sxs-lookup"><span data-stu-id="49507-185">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="49507-186">En la clase de modelo, anote la propiedad con un atributo `[Remote]` que apunte al método de acción de validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-186">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="49507-187">El atributo `[Remote]` está en el espacio de nombres `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="49507-187">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="49507-188">Campos adicionales</span><span class="sxs-lookup"><span data-stu-id="49507-188">Additional fields</span></span>

<span data-ttu-id="49507-189">La propiedad `AdditionalFields` del atributo `[Remote]` permite validar combinaciones de campos con los datos del servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-189">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="49507-190">Por ejemplo, si el modelo `User` tuviera las propiedades `FirstName` y `LastName`, podría interesarle comprobar que ningún usuario actual tuviera ya ese par de nombres.</span><span class="sxs-lookup"><span data-stu-id="49507-190">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="49507-191">En el siguiente ejemplo se muestra cómo usar `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="49507-191">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="49507-192">`AdditionalFields` podría configurarse explícitamente para las cadenas `"FirstName"` y `"LastName"`, pero al usar el operador [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) se simplifica la refactorización posterior.</span><span class="sxs-lookup"><span data-stu-id="49507-192">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="49507-193">El método de acción para esta validación debe aceptar los argumentos `firstName` y `lastName`:</span><span class="sxs-lookup"><span data-stu-id="49507-193">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="49507-194">Cuando el usuario escribe un nombre o un apellido, JavaScript realiza una llamada remota para comprobar si ese par de nombres ya existe.</span><span class="sxs-lookup"><span data-stu-id="49507-194">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="49507-195">Para validar dos o más campos adicionales, proporciónelos como una lista delimitada por comas.</span><span class="sxs-lookup"><span data-stu-id="49507-195">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="49507-196">Por ejemplo, para agregar una propiedad `MiddleName` al modelo, establezca el atributo `[Remote]` tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-196">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="49507-197">`AdditionalFields`, al igual que todos los argumentos de atributo, debe ser una expresión constante.</span><span class="sxs-lookup"><span data-stu-id="49507-197">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="49507-198">Por lo tanto, no use una [cadena interpolada](/dotnet/csharp/language-reference/keywords/interpolated-strings) ni llame a <xref:System.String.Join*> para inicializar `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="49507-198">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="49507-199">Alternativas a los atributos integrados</span><span class="sxs-lookup"><span data-stu-id="49507-199">Alternatives to built-in attributes</span></span>

<span data-ttu-id="49507-200">Si necesita una validación que no proporcionan los atributos integrados, puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-200">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="49507-201">[Crear atributos personalizados](#custom-attributes)</span><span class="sxs-lookup"><span data-stu-id="49507-201">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="49507-202">[Implementar IValidatableObject](#ivalidatableobject)</span><span class="sxs-lookup"><span data-stu-id="49507-202">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="49507-203">Atributos personalizados</span><span class="sxs-lookup"><span data-stu-id="49507-203">Custom attributes</span></span>

<span data-ttu-id="49507-204">Para los escenarios que no se controlan mediante los atributos de validación integrados, puede crear atributos de validación personalizados.</span><span class="sxs-lookup"><span data-stu-id="49507-204">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="49507-205">Cree una clase que herede de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> y reemplace el método <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="49507-205">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="49507-206">El método `IsValid` acepta un objeto denominado *value*, que es la entrada que se va a validar.</span><span class="sxs-lookup"><span data-stu-id="49507-206">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="49507-207">Una sobrecarga también acepta un objeto `ValidationContext`, que proporciona información adicional, como la instancia del modelo creada por el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-207">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="49507-208">El siguiente ejemplo valida que la fecha de lanzamiento de una película del género *Classic* no sea posterior a un año especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-208">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="49507-209">El atributo `[ClassicMovie]`:</span><span class="sxs-lookup"><span data-stu-id="49507-209">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="49507-210">Solo se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-210">Is only run on the server.</span></span>
* <span data-ttu-id="49507-211">En el caso de las películas clásicas, valida la fecha de lanzamiento:</span><span class="sxs-lookup"><span data-stu-id="49507-211">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="49507-212">La variable `movie` del ejemplo anterior representa un objeto `Movie` que contiene los datos del envío del formulario.</span><span class="sxs-lookup"><span data-stu-id="49507-212">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="49507-213">Si se produce un error de validación, se devuelve un `ValidationResult` con un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="49507-213">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="49507-214">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="49507-214">IValidatableObject</span></span>

<span data-ttu-id="49507-215">El ejemplo anterior solo funciona con tipos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="49507-215">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="49507-216">Otra opción para la validación del nivel de clase consiste en implementar `IValidatableObject` en la clase de modelo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-216">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="49507-217">Validación de nodo de nivel superior</span><span class="sxs-lookup"><span data-stu-id="49507-217">Top-level node validation</span></span>

<span data-ttu-id="49507-218">Los nodos de nivel superior incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-218">Top-level nodes include:</span></span>

* <span data-ttu-id="49507-219">Parámetros de acción</span><span class="sxs-lookup"><span data-stu-id="49507-219">Action parameters</span></span>
* <span data-ttu-id="49507-220">Propiedades de controlador</span><span class="sxs-lookup"><span data-stu-id="49507-220">Controller properties</span></span>
* <span data-ttu-id="49507-221">Parámetros de controlador de página</span><span class="sxs-lookup"><span data-stu-id="49507-221">Page handler parameters</span></span>
* <span data-ttu-id="49507-222">Propiedades de modelo de página</span><span class="sxs-lookup"><span data-stu-id="49507-222">Page model properties</span></span>

<span data-ttu-id="49507-223">Los nodos de nivel superior enlazados al modelo se validan además de la validación de las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="49507-223">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="49507-224">En el ejemplo siguiente de la aplicación de muestra, el método `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> para validar el parámetro de acción `phone`:</span><span class="sxs-lookup"><span data-stu-id="49507-224">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="49507-225">Los nodos de nivel superior pueden usar <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-225">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="49507-226">En el ejemplo siguiente de la aplicación de muestra, el método `CheckAge` especifica que el parámetro `age` debe estar enlazado desde la cadena de consulta al enviar el formulario:</span><span class="sxs-lookup"><span data-stu-id="49507-226">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="49507-227">En la página Comprobar edad (*CheckAge.cshtml*), hay dos formularios.</span><span class="sxs-lookup"><span data-stu-id="49507-227">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="49507-228">El primer formulario envía un valor `Age` de `99` como una cadena de parámetro de consulta: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="49507-228">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="49507-229">Al enviar un parámetro `age` con un formato correcto desde la cadena de consulta, el formulario se valida.</span><span class="sxs-lookup"><span data-stu-id="49507-229">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="49507-230">El segundo formulario de la página Comprobar edad envía el valor `Age` en el cuerpo de la solicitud, y se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-230">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="49507-231">Se produce un error en el enlace porque el parámetro `age` debe provenir de una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="49507-231">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="49507-232">Número máximo de errores</span><span class="sxs-lookup"><span data-stu-id="49507-232">Maximum errors</span></span>

<span data-ttu-id="49507-233">La validación se detiene cuando se alcanza el número máximo de errores (200 de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="49507-233">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="49507-234">Puede configurar este número con el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49507-234">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="49507-235">Recursividad máxima</span><span class="sxs-lookup"><span data-stu-id="49507-235">Maximum recursion</span></span>

<span data-ttu-id="49507-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> recorre el gráfico de objetos del modelo que se está validando.</span><span class="sxs-lookup"><span data-stu-id="49507-236"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="49507-237">En el caso de los modelos profundos o infinitamente recursivos, la validación podría causar un desbordamiento de pila.</span><span class="sxs-lookup"><span data-stu-id="49507-237">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="49507-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) proporciona una manera de detener pronto la validación si la recursividad del visitante supera la profundidad configurada.</span><span class="sxs-lookup"><span data-stu-id="49507-238">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="49507-239">El valor predeterminado de `MvcOptions.MaxValidationDepth` es 32.</span><span class="sxs-lookup"><span data-stu-id="49507-239">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="49507-240">Cortocircuito automático</span><span class="sxs-lookup"><span data-stu-id="49507-240">Automatic short-circuit</span></span>

<span data-ttu-id="49507-241">La validación cortocircuita (se omite) automáticamente si el gráfico de modelo no requiere validación.</span><span class="sxs-lookup"><span data-stu-id="49507-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="49507-242">Entre los objetos para los que el tiempo de ejecución omite la validación se incluyen las colecciones de elementos primitivos (como `byte[]`, `string[]` y `Dictionary<string, string>`) y gráficos de objeto complejo que no tienen los validadores.</span><span class="sxs-lookup"><span data-stu-id="49507-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="49507-243">Deshabilitación de la validación</span><span class="sxs-lookup"><span data-stu-id="49507-243">Disable validation</span></span>

<span data-ttu-id="49507-244">Para deshabilitar la validación:</span><span class="sxs-lookup"><span data-stu-id="49507-244">To disable validation:</span></span>

1. <span data-ttu-id="49507-245">Cree una implementación de `IObjectModelValidator` que no marque ningún campo como no válido.</span><span class="sxs-lookup"><span data-stu-id="49507-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="49507-246">Agregue el código siguiente a `Startup.ConfigureServices` para reemplazar la implementación predeterminada de `IObjectModelValidator` en el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="49507-247">Es posible que todavía vea errores de estado del modelo originados en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="49507-248">Validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-248">Client-side validation</span></span>

<span data-ttu-id="49507-249">La validación del lado cliente impide realizar el envío hasta que el formulario sea válido.</span><span class="sxs-lookup"><span data-stu-id="49507-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="49507-250">El botón Enviar ejecuta JavaScript para enviar el formulario o mostrar mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="49507-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="49507-251">La validación del lado cliente evita un recorrido de ida y vuelta innecesario en el servidor cuando hay errores de entrada en un formulario.</span><span class="sxs-lookup"><span data-stu-id="49507-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="49507-252">Las siguientes referencias de script de *_Layout.cshtml* y *_ValidationScriptsPartial.cshtml* admiten la validación del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="49507-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="49507-253">El script [Validación discreta de jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) es una biblioteca front-end personalizada de Microsoft que se basa en el conocido complemento [Validación de jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="49507-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="49507-254">Si no usa Validación discreta de jQuery, deberá codificar la misma lógica de validación dos veces: una vez en los atributos de validación del lado servidor en las propiedades del modelo y luego en los scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="49507-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="49507-255">En su lugar, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan los atributos de validación y escriben metadatos de las propiedades del modelo para representar atributos `data-` HTML 5 para los elementos de formulario que necesitan validación.</span><span class="sxs-lookup"><span data-stu-id="49507-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="49507-256">Validación discreta de jQuery analiza los atributos `data-` y pasa la lógica a jQuery Validate. De este modo, la lógica de validación del lado servidor se "copia" de manera eficaz en el cliente.</span><span class="sxs-lookup"><span data-stu-id="49507-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="49507-257">Puede mostrar errores de validación en el cliente mediante el uso de asistentes de etiquetas, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="49507-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="49507-258">Los anteriores asistentes de etiquetas representan el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="49507-258">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="49507-259">Tenga en cuenta que los atributos `data-` en los resultados HTML corresponden a los atributos de validación para la propiedad `Movie.ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="49507-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="49507-260">El atributo `data-val-required` contiene un mensaje de error que se muestra si el usuario no rellena el campo de fecha de estreno.</span><span class="sxs-lookup"><span data-stu-id="49507-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="49507-261">La Validación discreta de jQuery pasa este valor al método [`required()`](https://jqueryvalidation.org/required-method/) de la Validación de jQuery, que muestra un mensaje en el elemento **\<span>** que lo acompaña.</span><span class="sxs-lookup"><span data-stu-id="49507-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="49507-262">La validación del tipo de datos se basa en el tipo .NET de una propiedad, a menos que lo reemplace un atributo `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="49507-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="49507-263">Los exploradores tienen sus propios mensajes de error de predeterminados, pero el paquete de Validación discreta de jQuery Validate puede invalidar esos mensajes.</span><span class="sxs-lookup"><span data-stu-id="49507-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="49507-264">Los atributos y las subclases `[DataType]`, como `[EmailAddress]`, permiten especificar el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="49507-264">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="49507-265">Agregar validación a formularios dinámicos</span><span class="sxs-lookup"><span data-stu-id="49507-265">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="49507-266">Validación discreta de jQuery pasa los parámetros y la lógica de validación a jQuery Validate cuando la página se carga por primera vez.</span><span class="sxs-lookup"><span data-stu-id="49507-266">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="49507-267">Por lo tanto, la validación no funciona automáticamente en los formularios generados dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="49507-267">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="49507-268">Para habilitar la validación, hay que indicarle a Validación discreta de jQuery que analice el formulario dinámico inmediatamente después de su creación.</span><span class="sxs-lookup"><span data-stu-id="49507-268">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="49507-269">Por ejemplo, en el código siguiente se configura la validación del lado cliente en un formulario agregado mediante AJAX.</span><span class="sxs-lookup"><span data-stu-id="49507-269">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="49507-270">El método `$.validator.unobtrusive.parse()` acepta un selector de jQuery para su único argumento.</span><span class="sxs-lookup"><span data-stu-id="49507-270">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="49507-271">Este método indica a Validación discreta de jQuery que analice los atributos `data-` de formularios dentro de ese selector.</span><span class="sxs-lookup"><span data-stu-id="49507-271">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="49507-272">Después, los valores de estos atributos se pasan al complemento de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="49507-272">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="49507-273">Agregar validación a controles dinámicos</span><span class="sxs-lookup"><span data-stu-id="49507-273">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="49507-274">El método `$.validator.unobtrusive.parse()` funciona en todo el formulario, no en los controles individuales generados dinámicamente, como `<input>` y `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="49507-274">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="49507-275">Para volver a analizar el formulario, quite los datos de validación que se agregaron cuando el formulario se analizó anteriormente, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-275">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="49507-276">Validación del lado cliente personalizada</span><span class="sxs-lookup"><span data-stu-id="49507-276">Custom client-side validation</span></span>

<span data-ttu-id="49507-277">Para personalizar la validación del lado cliente, es necesario generar atributos HTML `data-` que funcionen con un adaptador personalizado de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="49507-277">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="49507-278">El siguiente ejemplo de código de adaptador se escribió para los atributos `[ClassicMovie]` y `[ClassicMovieWithClientValidator]` que se introdujeron anteriormente en este artículo:</span><span class="sxs-lookup"><span data-stu-id="49507-278">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="49507-279">Para obtener información sobre cómo escribir adaptadores, vea la [documentación de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="49507-279">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="49507-280">El uso de un adaptador para un campo determinado se desencadena mediante atributos `data-` que:</span><span class="sxs-lookup"><span data-stu-id="49507-280">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="49507-281">Marcan que el campo está sujeto a validación (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="49507-281">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="49507-282">Identifican un nombre de regla de validación y un texto de mensaje de error (por ejemplo, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="49507-282">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="49507-283">Proporcionan los parámetros adicionales que necesite el validador (por ejemplo, `data-val-rulename-param1="value"`).</span><span class="sxs-lookup"><span data-stu-id="49507-283">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="49507-284">En el ejemplo siguiente se muestran los atributos `data-` para el atributo `ClassicMovie` de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-284">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="49507-285">Como se indicó anteriormente, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan información procedente de los atributos de validación para representar atributos `data-`.</span><span class="sxs-lookup"><span data-stu-id="49507-285">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="49507-286">Hay dos opciones para escribir código que dé como resultado la creación de atributos HTML `data-` personalizados:</span><span class="sxs-lookup"><span data-stu-id="49507-286">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="49507-287">Puede crear una clase que derive de `AttributeAdapterBase<TAttribute>` y una clase que implemente `IValidationAttributeAdapterProvider` y, después, registrar el atributo y su adaptador en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-287">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="49507-288">Este método sigue el [principio de responsabilidad única](https://wikipedia.org/wiki/Single_responsibility_principle), ya que el código de validación relacionado con servidor y el relacionado con el cliente se encuentran en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="49507-288">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="49507-289">El adaptador también cuenta con una ventaja: debido a que está registrado en la inserción de dependencias, tiene a su disposición otros servicios de la inserción de dependencias si es necesario.</span><span class="sxs-lookup"><span data-stu-id="49507-289">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="49507-290">Puede implementar `IClientModelValidator` en la clase `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="49507-290">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="49507-291">Este método es adecuado si el atributo no realiza ninguna validación en el lado servidor y no necesita ningún servicio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-291">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="49507-292">AttributeAdapter para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-292">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="49507-293">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovie` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="49507-293">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="49507-294">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="49507-294">To add client validation by using this method:</span></span>

1. <span data-ttu-id="49507-295">Cree una clase de adaptador de atributo para el atributo de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-295">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="49507-296">Derive la clase de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="49507-296">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="49507-297">Cree un método `AddValidation` que agregue atributos `data-` a la salida representada, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-297">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="49507-298">Cree una clase de proveedor de adaptador que implemente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="49507-298">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="49507-299">En el método `GetAttributeAdapter`, pase el atributo personalizado al constructor del adaptador, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-299">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="49507-300">Registre el proveedor del adaptador para la inserción de dependencias en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49507-300">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="49507-301">IClientModelValidator para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-301">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="49507-302">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovieWithClientValidator` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="49507-302">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="49507-303">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="49507-303">To add client validation by using this method:</span></span>

* <span data-ttu-id="49507-304">En el atributo de validación personalizado, implemente la interfaz `IClientModelValidator` y cree un método `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="49507-304">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="49507-305">En el método `AddValidation`, agregue atributos `data-` para la validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-305">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="49507-306">Deshabilitación de la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-306">Disable client-side validation</span></span>

<span data-ttu-id="49507-307">El código siguiente deshabilita la validación de cliente en Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="49507-307">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="49507-308">Otras opciones para deshabilitar la validación del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="49507-308">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="49507-309">Convierta en comentario la referencia a `_ValidationScriptsPartial` en todos los archivos *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49507-309">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="49507-310">Quite el contenido del archivo *Pages\Shared\_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49507-310">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="49507-311">El enfoque anterior no impedirá la validación del lado cliente de la biblioteca de clases de Razor de identidad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49507-311">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="49507-312">Para más información, consulte <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="49507-312">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49507-313">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="49507-313">Additional resources</span></span>

* [<span data-ttu-id="49507-314">Espacio de nombres System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="49507-314">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="49507-315">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="49507-315">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="49507-316">En este artículo se explica cómo validar la entrada del usuario en una aplicación ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49507-316">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="49507-317">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="49507-317">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="49507-318">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="49507-318">Model state</span></span>

<span data-ttu-id="49507-319">El estado del modelo representa los errores que proceden de dos subsistemas: el enlace de modelos y la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-319">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="49507-320">Los errores que se originan en el [enlace de modelos](model-binding.md) suelen ser errores de conversión de datos (por ejemplo, se especifica una "x" en un campo que espera un entero).</span><span class="sxs-lookup"><span data-stu-id="49507-320">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="49507-321">La validación de modelos se produce después de enlace de modelos y notifica errores cuando los datos no se ajustan a las reglas de negocios (por ejemplo, se especifica un 0 en un campo que espera una clasificación entre 1 y 5).</span><span class="sxs-lookup"><span data-stu-id="49507-321">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="49507-322">Tanto el enlace como la validación de modelos se producen antes de la ejecución de una acción de controlador o un método de controlador de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49507-322">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="49507-323">En el caso de las aplicaciones web, la aplicación es responsable de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="49507-323">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="49507-324">Normalmente, las aplicaciones web vuelven a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="49507-324">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="49507-325">Los controladores de Web API no tienen que comprobar `ModelState.IsValid` si tienen el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="49507-325">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="49507-326">En ese caso, se devuelve una respuesta HTTP 400 automática que contiene los detalles del error cuando el estado del modelo no es válido.</span><span class="sxs-lookup"><span data-stu-id="49507-326">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="49507-327">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="49507-327">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="49507-328">Nueva ejecución de la validación</span><span class="sxs-lookup"><span data-stu-id="49507-328">Rerun validation</span></span>

<span data-ttu-id="49507-329">La validación es automática, pero tal vez le interese repetirla manualmente.</span><span class="sxs-lookup"><span data-stu-id="49507-329">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="49507-330">Por ejemplo, tal vez haya calculado el valor de una propiedad y quiera volver a ejecutar la validación después de establecer la propiedad en el valor calculado.</span><span class="sxs-lookup"><span data-stu-id="49507-330">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="49507-331">Para volver a ejecutar la validación, llame al método `TryValidateModel`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="49507-331">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="49507-332">Atributos de validación</span><span class="sxs-lookup"><span data-stu-id="49507-332">Validation attributes</span></span>

<span data-ttu-id="49507-333">Los atributos de validación permiten especificar reglas de validación para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="49507-333">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="49507-334">En el ejemplo siguiente de la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) se muestra una clase de modelo anotada con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-334">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="49507-335">El atributo `[ClassicMovie]` es un atributo de validación personalizado y los demás están integrados.</span><span class="sxs-lookup"><span data-stu-id="49507-335">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="49507-336">No se muestra `[ClassicMovie2]`, que indica una manera alternativa de implementar un atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-336">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="49507-337">Atributos integrados</span><span class="sxs-lookup"><span data-stu-id="49507-337">Built-in attributes</span></span>

<span data-ttu-id="49507-338">Entre los atributos de validación integrados se incluyen:</span><span class="sxs-lookup"><span data-stu-id="49507-338">Built-in validation attributes include:</span></span>

* <span data-ttu-id="49507-339">`[CreditCard]`: valida que la propiedad tenga formato de tarjeta de crédito.</span><span class="sxs-lookup"><span data-stu-id="49507-339">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="49507-340">`[Compare]`: valida que dos propiedades de un modelo coincidan.</span><span class="sxs-lookup"><span data-stu-id="49507-340">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="49507-341">Por ejemplo, el archivo *Register.cshtml.cs* usa `[Compare]` para validar que ambas contraseñas escritas coincidan.</span><span class="sxs-lookup"><span data-stu-id="49507-341">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="49507-342">[Identidad de scaffolding](xref:security/authentication/scaffold-identity) para ver el código del Registro.</span><span class="sxs-lookup"><span data-stu-id="49507-342">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="49507-343">`[EmailAddress]`: valida que la propiedad tenga formato de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49507-343">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="49507-344">`[Phone]`: valida que la propiedad tenga formato de número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="49507-344">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="49507-345">`[Range]`: valida que el valor de propiedad se encuentre dentro de un intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-345">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="49507-346">`[RegularExpression]`: valida que el valor de propiedad coincida con una expresión regular especificada.</span><span class="sxs-lookup"><span data-stu-id="49507-346">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="49507-347">`[Required]`: valida que el campo no sea NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-347">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="49507-348">Vea [Atributo [Required]](#required-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-348">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="49507-349">`[StringLength]`: valida que un valor de propiedad de cadena no supere un límite de longitud especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-349">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="49507-350">`[Url]`: valida que la propiedad tenga un formato de URL.</span><span class="sxs-lookup"><span data-stu-id="49507-350">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="49507-351">`[Remote]`: valida la entrada en el cliente mediante una llamada a un método de acción en el servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-351">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="49507-352">Vea [Atributo [Remote]](#remote-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-352">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="49507-353">En el espacio de nombres [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) encontrará una lista completa de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-353">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="49507-354">Mensajes de error</span><span class="sxs-lookup"><span data-stu-id="49507-354">Error messages</span></span>

<span data-ttu-id="49507-355">Los atributos de validación permiten especificar el mensaje de error que se mostrará para una entrada no válida.</span><span class="sxs-lookup"><span data-stu-id="49507-355">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="49507-356">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-356">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="49507-357">Internamente, los atributos llaman a `String.Format` con un marcador de posición para el nombre de campo y, en ocasiones, marcadores de posición adicionales.</span><span class="sxs-lookup"><span data-stu-id="49507-357">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="49507-358">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-358">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="49507-359">Cuando se aplica a una propiedad `Name`, el mensaje de error creado por el código anterior sería "La longitud del nombre debe estar entre 6 y 8".</span><span class="sxs-lookup"><span data-stu-id="49507-359">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="49507-360">Para averiguar qué parámetros se pasan a `String.Format` para el mensaje de error de un atributo determinado, vea el [código fuente de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="49507-360">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="49507-361">Atributo [Required]</span><span class="sxs-lookup"><span data-stu-id="49507-361">[Required] attribute</span></span>

<span data-ttu-id="49507-362">De forma predeterminada, el sistema de validación trata las propiedades o los parámetros que no aceptan valores NULL como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-362">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="49507-363">Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) como `decimal` y `int` no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-363">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="49507-364">Validación de [Required] en el servidor</span><span class="sxs-lookup"><span data-stu-id="49507-364">[Required] validation on the server</span></span>

<span data-ttu-id="49507-365">En el servidor, si la propiedad es NULL, se considera que falta un valor requerido.</span><span class="sxs-lookup"><span data-stu-id="49507-365">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="49507-366">Un campo que no acepta valores NULL siempre es válido, y el mensaje de error del atributo [Required] no se muestra nunca.</span><span class="sxs-lookup"><span data-stu-id="49507-366">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="49507-367">Aun así, el enlace de modelos para una propiedad que no acepta valores NULL podría fallar, lo que genera un mensaje de error como `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="49507-367">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="49507-368">Para especificar un mensaje de error personalizado para la validación del lado servidor de tipos que no aceptan valores NULL, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="49507-368">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="49507-369">Haga que el campo acepte valores NULL (por ejemplo, `decimal?` en lugar de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="49507-369">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="49507-370">Los tipos de valor [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) se tratan como tipos estándar que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-370">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="49507-371">Especifique el mensaje de error predeterminado que el enlace de modelos va a usar, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-371">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="49507-372">Para obtener más información sobre los errores de enlace de modelos para los que se pueden establecer mensajes predeterminados, vea <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="49507-372">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="49507-373">Validación de [Required] en el cliente</span><span class="sxs-lookup"><span data-stu-id="49507-373">[Required] validation on the client</span></span>

<span data-ttu-id="49507-374">Las cadenas y los tipos que no aceptan valores NULL se tratan de forma diferente en el cliente, en comparación con el servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-374">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="49507-375">En el cliente:</span><span class="sxs-lookup"><span data-stu-id="49507-375">On the client:</span></span>

* <span data-ttu-id="49507-376">Un valor se considera presente solo si se especifica una entrada para él.</span><span class="sxs-lookup"><span data-stu-id="49507-376">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="49507-377">Por lo tanto, la validación del lado cliente controla los tipos que no aceptan valores NULL del mismo modo que los tipos que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="49507-377">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="49507-378">El método [required](https://jqueryvalidation.org/required-method/) de jQuery Validate considera que un espacio en blanco en un campo de cadena es una entrada válida.</span><span class="sxs-lookup"><span data-stu-id="49507-378">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="49507-379">La validación del lado servidor considera que un campo de cadena necesario no es válido si solo se especifica un espacio en blanco.</span><span class="sxs-lookup"><span data-stu-id="49507-379">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="49507-380">Como se indicó anteriormente, los tipos que no aceptan valores NULL se tratan como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-380">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="49507-381">Esto significa que se obtiene la validación del lado cliente incluso si no se aplica el atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="49507-381">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="49507-382">Si no usa el atributo, aparecerá un mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49507-382">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="49507-383">Para especificar un mensaje de error personalizado, use el atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-383">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="49507-384">Atributo [Remote]</span><span class="sxs-lookup"><span data-stu-id="49507-384">[Remote] attribute</span></span>

<span data-ttu-id="49507-385">El atributo `[Remote]` implementa la validación del lado cliente que requiere llamar a un método en el servidor para determinar si la entrada del campo es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-385">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="49507-386">Por ejemplo, la aplicación podría tener que comprobar si un nombre de usuario ya está en uso.</span><span class="sxs-lookup"><span data-stu-id="49507-386">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="49507-387">Para implementar la validación remota:</span><span class="sxs-lookup"><span data-stu-id="49507-387">To implement remote validation:</span></span>

1. <span data-ttu-id="49507-388">Cree un método de acción para que lo llame JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49507-388">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="49507-389">El método [remote](https://jqueryvalidation.org/remote-method/) de jQuery Validate espera una respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="49507-389">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="49507-390">`"true"` significa que los datos de entrada son válidos.</span><span class="sxs-lookup"><span data-stu-id="49507-390">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="49507-391">`"false"`, `undefined` o `null` significan que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-391">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="49507-392">Muestre el mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="49507-392">Display the default error message.</span></span>
   * <span data-ttu-id="49507-393">Cualquier otra cadena significa que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="49507-393">Any other string means the input is invalid.</span></span> <span data-ttu-id="49507-394">Muestre la cadena como un mensaje de error personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-394">Display the string as a custom error message.</span></span>

   <span data-ttu-id="49507-395">A continuación encontrará un ejemplo de un método de acción que devuelve un mensaje de error personalizado:</span><span class="sxs-lookup"><span data-stu-id="49507-395">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="49507-396">En la clase de modelo, anote la propiedad con un atributo `[Remote]` que apunte al método de acción de validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-396">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="49507-397">El atributo `[Remote]` está en el espacio de nombres `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="49507-397">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="49507-398">Instale el paquete de NuGet [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) si no usa `Microsoft.AspNetCore.App` o el metapaquete `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="49507-398">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="49507-399">Campos adicionales</span><span class="sxs-lookup"><span data-stu-id="49507-399">Additional fields</span></span>

<span data-ttu-id="49507-400">La propiedad `AdditionalFields` del atributo `[Remote]` permite validar combinaciones de campos con los datos del servidor.</span><span class="sxs-lookup"><span data-stu-id="49507-400">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="49507-401">Por ejemplo, si el modelo `User` tuviera las propiedades `FirstName` y `LastName`, podría interesarle comprobar que ningún usuario actual tuviera ya ese par de nombres.</span><span class="sxs-lookup"><span data-stu-id="49507-401">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="49507-402">En el siguiente ejemplo se muestra cómo usar `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="49507-402">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="49507-403">`AdditionalFields` podría configurarse explícitamente para las cadenas `"FirstName"` y `"LastName"`, pero al usar el operador [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) se simplifica la refactorización posterior.</span><span class="sxs-lookup"><span data-stu-id="49507-403">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="49507-404">El método de acción para esta validación debe aceptar los argumentos de nombre y apellido:</span><span class="sxs-lookup"><span data-stu-id="49507-404">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="49507-405">Cuando el usuario escribe un nombre o un apellido, JavaScript realiza una llamada remota para comprobar si ese par de nombres ya existe.</span><span class="sxs-lookup"><span data-stu-id="49507-405">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="49507-406">Para validar dos o más campos adicionales, proporciónelos como una lista delimitada por comas.</span><span class="sxs-lookup"><span data-stu-id="49507-406">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="49507-407">Por ejemplo, para agregar una propiedad `MiddleName` al modelo, establezca el atributo `[Remote]` tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-407">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="49507-408">`AdditionalFields`, al igual que todos los argumentos de atributo, debe ser una expresión constante.</span><span class="sxs-lookup"><span data-stu-id="49507-408">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="49507-409">Por lo tanto, no use una [cadena interpolada](/dotnet/csharp/language-reference/keywords/interpolated-strings) ni llame a <xref:System.String.Join*> para inicializar `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="49507-409">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="49507-410">Alternativas a los atributos integrados</span><span class="sxs-lookup"><span data-stu-id="49507-410">Alternatives to built-in attributes</span></span>

<span data-ttu-id="49507-411">Si necesita una validación que no proporcionan los atributos integrados, puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-411">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="49507-412">[Crear atributos personalizados](#custom-attributes)</span><span class="sxs-lookup"><span data-stu-id="49507-412">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="49507-413">[Implementar IValidatableObject](#ivalidatableobject)</span><span class="sxs-lookup"><span data-stu-id="49507-413">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="49507-414">Atributos personalizados</span><span class="sxs-lookup"><span data-stu-id="49507-414">Custom attributes</span></span>

<span data-ttu-id="49507-415">Para los escenarios que no se controlan mediante los atributos de validación integrados, puede crear atributos de validación personalizados.</span><span class="sxs-lookup"><span data-stu-id="49507-415">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="49507-416">Cree una clase que herede de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> y reemplace el método <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="49507-416">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="49507-417">El método `IsValid` acepta un objeto denominado *value*, que es la entrada que se va a validar.</span><span class="sxs-lookup"><span data-stu-id="49507-417">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="49507-418">Una sobrecarga también acepta un objeto `ValidationContext`, que proporciona información adicional, como la instancia del modelo creada por el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-418">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="49507-419">El siguiente ejemplo valida que la fecha de lanzamiento de una película del género *Classic* no sea posterior a un año especificado.</span><span class="sxs-lookup"><span data-stu-id="49507-419">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="49507-420">El atributo `[ClassicMovie2]` comprueba primero el género y continúa únicamente si es *Classic*.</span><span class="sxs-lookup"><span data-stu-id="49507-420">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="49507-421">Para las películas que se identifican como clásicos, comprueba la fecha de lanzamiento a fin de asegurarse de que no sea posterior al límite que se ha pasado al constructor de atributo.</span><span class="sxs-lookup"><span data-stu-id="49507-421">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="49507-422">La variable `movie` del ejemplo anterior representa un objeto `Movie` que contiene los datos del envío del formulario.</span><span class="sxs-lookup"><span data-stu-id="49507-422">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="49507-423">El método `IsValid` comprueba la fecha y el género.</span><span class="sxs-lookup"><span data-stu-id="49507-423">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="49507-424">Si la validación es correcta, `IsValid` devuelve un código `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="49507-424">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="49507-425">Si se produce un error de validación, se devuelve un `ValidationResult` con un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="49507-425">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="49507-426">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="49507-426">IValidatableObject</span></span>

<span data-ttu-id="49507-427">El ejemplo anterior solo funciona con tipos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="49507-427">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="49507-428">Otra opción para la validación del nivel de clase consiste en implementar `IValidatableObject` en la clase de modelo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-428">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="49507-429">Validación de nodo de nivel superior</span><span class="sxs-lookup"><span data-stu-id="49507-429">Top-level node validation</span></span>

<span data-ttu-id="49507-430">Los nodos de nivel superior incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-430">Top-level nodes include:</span></span>

* <span data-ttu-id="49507-431">Parámetros de acción</span><span class="sxs-lookup"><span data-stu-id="49507-431">Action parameters</span></span>
* <span data-ttu-id="49507-432">Propiedades de controlador</span><span class="sxs-lookup"><span data-stu-id="49507-432">Controller properties</span></span>
* <span data-ttu-id="49507-433">Parámetros de controlador de página</span><span class="sxs-lookup"><span data-stu-id="49507-433">Page handler parameters</span></span>
* <span data-ttu-id="49507-434">Propiedades de modelo de página</span><span class="sxs-lookup"><span data-stu-id="49507-434">Page model properties</span></span>

<span data-ttu-id="49507-435">Los nodos de nivel superior enlazados al modelo se validan además de la validación de las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="49507-435">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="49507-436">En el ejemplo siguiente de la aplicación de muestra, el método `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> para validar el parámetro de acción `phone`:</span><span class="sxs-lookup"><span data-stu-id="49507-436">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="49507-437">Los nodos de nivel superior pueden usar <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-437">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="49507-438">En el ejemplo siguiente de la aplicación de muestra, el método `CheckAge` especifica que el parámetro `age` debe estar enlazado desde la cadena de consulta al enviar el formulario:</span><span class="sxs-lookup"><span data-stu-id="49507-438">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="49507-439">En la página Comprobar edad (*CheckAge.cshtml*), hay dos formularios.</span><span class="sxs-lookup"><span data-stu-id="49507-439">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="49507-440">El primer formulario envía un valor `Age` de `99` como una cadena de consulta: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="49507-440">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="49507-441">Al enviar un parámetro `age` con un formato correcto desde la cadena de consulta, el formulario se valida.</span><span class="sxs-lookup"><span data-stu-id="49507-441">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="49507-442">El segundo formulario de la página Comprobar edad envía el valor `Age` en el cuerpo de la solicitud, y se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="49507-442">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="49507-443">Se produce un error en el enlace porque el parámetro `age` debe provenir de una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="49507-443">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="49507-444">Cuando se ejecuta con `CompatibilityVersion.Version_2_1` o versiones posteriores, la validación de nodo de nivel superior está habilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49507-444">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="49507-445">En caso contrario, la validación del nodo de nivel superior está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="49507-445">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="49507-446">La opción predeterminada se puede invalidar si se establece la propiedad <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> en (`Startup.ConfigureServices`), como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="49507-446">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="49507-447">Número máximo de errores</span><span class="sxs-lookup"><span data-stu-id="49507-447">Maximum errors</span></span>

<span data-ttu-id="49507-448">La validación se detiene cuando se alcanza el número máximo de errores (200 de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="49507-448">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="49507-449">Puede configurar este número con el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49507-449">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="49507-450">Recursividad máxima</span><span class="sxs-lookup"><span data-stu-id="49507-450">Maximum recursion</span></span>

<span data-ttu-id="49507-451"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> recorre el gráfico de objetos del modelo que se está validando.</span><span class="sxs-lookup"><span data-stu-id="49507-451"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="49507-452">En el caso de los modelos muy profundos o infinitamente recursivos, la validación podría causar un desbordamiento de pila.</span><span class="sxs-lookup"><span data-stu-id="49507-452">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="49507-453">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) proporciona una manera de detener pronto la validación si la recursividad del visitante supera la profundidad configurada.</span><span class="sxs-lookup"><span data-stu-id="49507-453">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="49507-454">El valor predeterminado de `MvcOptions.MaxValidationDepth` es 32 cuando se ejecuta con `CompatibilityVersion.Version_2_2` o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="49507-454">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="49507-455">Para las versiones anteriores, el valor es NULL, lo que significa que no hay restricción de profundidad.</span><span class="sxs-lookup"><span data-stu-id="49507-455">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="49507-456">Cortocircuito automático</span><span class="sxs-lookup"><span data-stu-id="49507-456">Automatic short-circuit</span></span>

<span data-ttu-id="49507-457">La validación cortocircuita (se omite) automáticamente si el gráfico de modelo no requiere validación.</span><span class="sxs-lookup"><span data-stu-id="49507-457">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="49507-458">Entre los objetos para los que el tiempo de ejecución omite la validación se incluyen las colecciones de elementos primitivos (como `byte[]`, `string[]` y `Dictionary<string, string>`) y gráficos de objeto complejo que no tienen los validadores.</span><span class="sxs-lookup"><span data-stu-id="49507-458">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="49507-459">Deshabilitación de la validación</span><span class="sxs-lookup"><span data-stu-id="49507-459">Disable validation</span></span>

<span data-ttu-id="49507-460">Para deshabilitar la validación:</span><span class="sxs-lookup"><span data-stu-id="49507-460">To disable validation:</span></span>

1. <span data-ttu-id="49507-461">Cree una implementación de `IObjectModelValidator` que no marque ningún campo como no válido.</span><span class="sxs-lookup"><span data-stu-id="49507-461">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="49507-462">Agregue el código siguiente a `Startup.ConfigureServices` para reemplazar la implementación predeterminada de `IObjectModelValidator` en el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-462">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="49507-463">Es posible que todavía vea errores de estado del modelo originados en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="49507-463">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="49507-464">Validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-464">Client-side validation</span></span>

<span data-ttu-id="49507-465">La validación del lado cliente impide realizar el envío hasta que el formulario sea válido.</span><span class="sxs-lookup"><span data-stu-id="49507-465">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="49507-466">El botón Enviar ejecuta JavaScript para enviar el formulario o mostrar mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="49507-466">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="49507-467">La validación del lado cliente evita un recorrido de ida y vuelta innecesario en el servidor cuando hay errores de entrada en un formulario.</span><span class="sxs-lookup"><span data-stu-id="49507-467">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="49507-468">Las siguientes referencias de script de *_Layout.cshtml* y *_ValidationScriptsPartial.cshtml* admiten la validación del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="49507-468">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="49507-469">El script [Validación discreta de jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) es una biblioteca front-end personalizada de Microsoft que se basa en el conocido complemento [Validación de jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="49507-469">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="49507-470">Si no usa Validación discreta de jQuery, deberá codificar la misma lógica de validación dos veces: una vez en los atributos de validación del lado servidor en las propiedades del modelo y luego en los scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="49507-470">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="49507-471">En su lugar, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan los atributos de validación y escriben metadatos de las propiedades del modelo para representar atributos `data-` HTML 5 para los elementos de formulario que necesitan validación.</span><span class="sxs-lookup"><span data-stu-id="49507-471">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="49507-472">Validación discreta de jQuery analiza los atributos `data-` y pasa la lógica a jQuery Validate. De este modo, la lógica de validación del lado servidor se "copia" de manera eficaz en el cliente.</span><span class="sxs-lookup"><span data-stu-id="49507-472">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="49507-473">Puede mostrar errores de validación en el cliente mediante el uso de asistentes de etiquetas, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="49507-473">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="49507-474">Los anteriores asistentes de etiquetas representan el siguiente código HTML.</span><span class="sxs-lookup"><span data-stu-id="49507-474">The preceding tag helpers render the following HTML.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="49507-475">Tenga en cuenta que los atributos `data-` en los resultados HTML corresponden a los atributos de validación para la propiedad `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="49507-475">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="49507-476">El atributo `data-val-required` contiene un mensaje de error que se muestra si el usuario no rellena el campo de fecha de estreno.</span><span class="sxs-lookup"><span data-stu-id="49507-476">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="49507-477">La Validación discreta de jQuery pasa este valor al método [`required()`](https://jqueryvalidation.org/required-method/) de la Validación de jQuery, que muestra un mensaje en el elemento **\<span>** que lo acompaña.</span><span class="sxs-lookup"><span data-stu-id="49507-477">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="49507-478">La validación del tipo de datos se basa en el tipo .NET de una propiedad, a menos que lo reemplace un atributo `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="49507-478">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="49507-479">Los exploradores tienen sus propios mensajes de error de predeterminados, pero el paquete de Validación discreta de jQuery Validate puede invalidar esos mensajes.</span><span class="sxs-lookup"><span data-stu-id="49507-479">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="49507-480">Los atributos y las subclases `[DataType]`, como `[EmailAddress]`, permiten especificar el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="49507-480">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="49507-481">Agregar validación a formularios dinámicos</span><span class="sxs-lookup"><span data-stu-id="49507-481">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="49507-482">Validación discreta de jQuery pasa los parámetros y la lógica de validación a jQuery Validate cuando la página se carga por primera vez.</span><span class="sxs-lookup"><span data-stu-id="49507-482">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="49507-483">Por lo tanto, la validación no funciona automáticamente en los formularios generados dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="49507-483">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="49507-484">Para habilitar la validación, hay que indicarle a Validación discreta de jQuery que analice el formulario dinámico inmediatamente después de su creación.</span><span class="sxs-lookup"><span data-stu-id="49507-484">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="49507-485">Por ejemplo, en el código siguiente se configura la validación del lado cliente en un formulario agregado mediante AJAX.</span><span class="sxs-lookup"><span data-stu-id="49507-485">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="49507-486">El método `$.validator.unobtrusive.parse()` acepta un selector de jQuery para su único argumento.</span><span class="sxs-lookup"><span data-stu-id="49507-486">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="49507-487">Este método indica a Validación discreta de jQuery que analice los atributos `data-` de formularios dentro de ese selector.</span><span class="sxs-lookup"><span data-stu-id="49507-487">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="49507-488">Después, los valores de estos atributos se pasan al complemento de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="49507-488">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="49507-489">Agregar validación a controles dinámicos</span><span class="sxs-lookup"><span data-stu-id="49507-489">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="49507-490">El método `$.validator.unobtrusive.parse()` funciona en todo el formulario, no en los controles individuales generados dinámicamente, como `<input>` y `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="49507-490">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="49507-491">Para volver a analizar el formulario, quite los datos de validación que se agregaron cuando el formulario se analizó anteriormente, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-491">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="custom-client-side-validation"></a><span data-ttu-id="49507-492">Validación del lado cliente personalizada</span><span class="sxs-lookup"><span data-stu-id="49507-492">Custom client-side validation</span></span>

<span data-ttu-id="49507-493">Para personalizar la validación del lado cliente, es necesario generar atributos HTML `data-` que funcionen con un adaptador personalizado de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="49507-493">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="49507-494">El siguiente ejemplo de código de adaptador se escribió para los atributos `ClassicMovie` y `ClassicMovie2` que se introdujeron anteriormente en este artículo:</span><span class="sxs-lookup"><span data-stu-id="49507-494">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="49507-495">Para obtener información sobre cómo escribir adaptadores, vea la [documentación de jQuery Validate](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="49507-495">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="49507-496">El uso de un adaptador para un campo determinado se desencadena mediante atributos `data-` que:</span><span class="sxs-lookup"><span data-stu-id="49507-496">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="49507-497">Marcan que el campo está sujeto a validación (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="49507-497">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="49507-498">Identifican un nombre de regla de validación y un texto de mensaje de error (por ejemplo, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="49507-498">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="49507-499">Proporcionan los parámetros adicionales que necesite el validador (por ejemplo, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="49507-499">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="49507-500">En el ejemplo siguiente se muestran los atributos `data-` para el atributo `ClassicMovie` de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-500">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="49507-501">Como se indicó anteriormente, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan información procedente de los atributos de validación para representar atributos `data-`.</span><span class="sxs-lookup"><span data-stu-id="49507-501">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="49507-502">Hay dos opciones para escribir código que dé como resultado la creación de atributos HTML `data-` personalizados:</span><span class="sxs-lookup"><span data-stu-id="49507-502">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="49507-503">Puede crear una clase que derive de `AttributeAdapterBase<TAttribute>` y una clase que implemente `IValidationAttributeAdapterProvider` y, después, registrar el atributo y su adaptador en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-503">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="49507-504">Este método sigue el [principio de responsabilidad única](https://wikipedia.org/wiki/Single_responsibility_principle), ya que el código de validación relacionado con servidor y el relacionado con el cliente se encuentran en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="49507-504">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="49507-505">El adaptador también cuenta con una ventaja: debido a que está registrado en la inserción de dependencias, tiene a su disposición otros servicios de la inserción de dependencias si es necesario.</span><span class="sxs-lookup"><span data-stu-id="49507-505">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="49507-506">Puede implementar `IClientModelValidator` en la clase `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="49507-506">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="49507-507">Este método es adecuado si el atributo no realiza ninguna validación en el lado servidor y no necesita ningún servicio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="49507-507">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="49507-508">AttributeAdapter para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-508">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="49507-509">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovie` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="49507-509">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="49507-510">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="49507-510">To add client validation by using this method:</span></span>

1. <span data-ttu-id="49507-511">Cree una clase de adaptador de atributo para el atributo de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="49507-511">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="49507-512">Derive la clase de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="49507-512">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="49507-513">Cree un método `AddValidation` que agregue atributos `data-` a la salida representada, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-513">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="49507-514">Cree una clase de proveedor de adaptador que implemente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="49507-514">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="49507-515">En el método `GetAttributeAdapter`, pase el atributo personalizado al constructor del adaptador, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49507-515">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="49507-516">Registre el proveedor del adaptador para la inserción de dependencias en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="49507-516">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="49507-517">IClientModelValidator para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-517">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="49507-518">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovie2` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="49507-518">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="49507-519">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="49507-519">To add client validation by using this method:</span></span>

* <span data-ttu-id="49507-520">En el atributo de validación personalizado, implemente la interfaz `IClientModelValidator` y cree un método `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="49507-520">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="49507-521">En el método `AddValidation`, agregue atributos `data-` para la validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49507-521">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="49507-522">Deshabilitación de la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="49507-522">Disable client-side validation</span></span>

<span data-ttu-id="49507-523">El código siguiente deshabilita la validación de cliente en las vistas de MVC:</span><span class="sxs-lookup"><span data-stu-id="49507-523">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="49507-524">Y en Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="49507-524">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="49507-525">Otra opción para deshabilitar la validación de cliente consiste en marcar como comentario la referencia a `_ValidationScriptsPartial` en el archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49507-525">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49507-526">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="49507-526">Additional resources</span></span>

* [<span data-ttu-id="49507-527">Espacio de nombres System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="49507-527">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="49507-528">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="49507-528">Model Binding</span></span>](model-binding.md)

::: moniker-end
