---
title: Validación de modelos en ASP.NET Core MVC
author: tdykstra
description: Obtenga información sobre la validación de modelos en ASP.NET Core MVC y Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
monikerRange: '>= aspnetcore-2.1'
uid: mvc/models/validation
ms.openlocfilehash: 1ae3c20478b02d6f654e65fdf34c88e1ffb837f8
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468742"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="12843-103">Validación de modelos en ASP.NET Core MVC y Razor Pages</span><span class="sxs-lookup"><span data-stu-id="12843-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

<span data-ttu-id="12843-104">En este artículo se explica cómo validar la entrada del usuario en una aplicación ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="12843-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="12843-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="12843-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="12843-106">Estado del modelo</span><span class="sxs-lookup"><span data-stu-id="12843-106">Model state</span></span>

<span data-ttu-id="12843-107">El estado del modelo representa los errores que proceden de dos subsistemas: el enlace de modelos y la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="12843-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="12843-108">Los errores que se originan en el [enlace de modelos](model-binding.md) suelen ser errores de conversión de datos (por ejemplo, se especifica una "x" en un campo que espera un entero).</span><span class="sxs-lookup"><span data-stu-id="12843-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="12843-109">La validación de modelos se produce después de enlace de modelos y notifica errores cuando los datos no se ajustan a las reglas de negocios (por ejemplo, se especifica un 0 en un campo que espera una clasificación entre 1 y 5).</span><span class="sxs-lookup"><span data-stu-id="12843-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="12843-110">Tanto el enlace como la validación de modelos se producen antes de la ejecución de una acción de controlador o un método de controlador de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="12843-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="12843-111">En el caso de las aplicaciones web, la aplicación es responsable de inspeccionar `ModelState.IsValid` y reaccionar de manera apropiada.</span><span class="sxs-lookup"><span data-stu-id="12843-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="12843-112">Normalmente, las aplicaciones web vuelven a mostrar la página con un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="12843-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="12843-113">Los controladores de Web API no tienen que comprobar `ModelState.IsValid` si tienen el atributo `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="12843-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="12843-114">En ese caso, se devuelve una respuesta HTTP 400 automática que contiene los detalles del problema cuando el estado del modelo no es válido.</span><span class="sxs-lookup"><span data-stu-id="12843-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="12843-115">Para obtener más información, consulte [Respuestas HTTP 400 automáticas](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="12843-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="12843-116">Nueva ejecución de la validación</span><span class="sxs-lookup"><span data-stu-id="12843-116">Rerun validation</span></span>

<span data-ttu-id="12843-117">La validación es automática, pero tal vez le interese repetirla manualmente.</span><span class="sxs-lookup"><span data-stu-id="12843-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="12843-118">Por ejemplo, tal vez haya calculado el valor de una propiedad y quiera volver a ejecutar la validación después de establecer la propiedad en el valor calculado.</span><span class="sxs-lookup"><span data-stu-id="12843-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="12843-119">Para volver a ejecutar la validación, llame al método `TryValidateModel`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="12843-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="12843-120">Atributos de validación</span><span class="sxs-lookup"><span data-stu-id="12843-120">Validation attributes</span></span>

<span data-ttu-id="12843-121">Los atributos de validación permiten especificar reglas de validación para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="12843-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="12843-122">En el ejemplo siguiente de la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) se muestra una clase de modelo anotada con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="12843-122">The following example from [the sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="12843-123">El atributo `[ClassicMovie]` es un atributo de validación personalizado y los demás están integrados.</span><span class="sxs-lookup"><span data-stu-id="12843-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="12843-124">(No se muestra `[ClassicMovie2]`, que indica una manera alternativa de implementar un atributo personalizado).</span><span class="sxs-lookup"><span data-stu-id="12843-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="12843-125">Atributos integrados</span><span class="sxs-lookup"><span data-stu-id="12843-125">Built-in attributes</span></span>

<span data-ttu-id="12843-126">Estos son algunos de los atributos de validación integrados:</span><span class="sxs-lookup"><span data-stu-id="12843-126">Here are some of the built-in validation attributes:</span></span>

* `[CreditCard]`<span data-ttu-id="12843-127">: valida que la propiedad tenga formato de tarjeta de crédito.</span><span class="sxs-lookup"><span data-stu-id="12843-127">: Validates that the property has a credit card format.</span></span>
* `[Compare]`<span data-ttu-id="12843-128">: valida que dos propiedades de un modelo coincidan.</span><span class="sxs-lookup"><span data-stu-id="12843-128">: Validates that two properties in a model match.</span></span>
* `[EmailAddress]`<span data-ttu-id="12843-129">: valida que la propiedad tenga formato de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="12843-129">: Validates that the property has an email format.</span></span>
* `[Phone]`<span data-ttu-id="12843-130">: valida que la propiedad tenga formato de número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="12843-130">: Validates that the property has a telephone number format.</span></span>
* `[Range]`<span data-ttu-id="12843-131">: valida que el valor de propiedad se encuentre dentro de un intervalo especificado.</span><span class="sxs-lookup"><span data-stu-id="12843-131">: Validates that the property value falls within a specified range.</span></span>
* `[RegularExpression]`<span data-ttu-id="12843-132">: valida que el valor de propiedad coincida con una expresión regular especificada.</span><span class="sxs-lookup"><span data-stu-id="12843-132">: Validates that the property value matches a specified regular expression.</span></span>
* `[Required]`<span data-ttu-id="12843-133">: valida que el campo no sea NULL.</span><span class="sxs-lookup"><span data-stu-id="12843-133">: Validates that the field is not null.</span></span> <span data-ttu-id="12843-134">Vea [Atributo [Required]](#required-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="12843-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* `[StringLength]`<span data-ttu-id="12843-135">: valida que un valor de propiedad de cadena no supere un límite de longitud especificado.</span><span class="sxs-lookup"><span data-stu-id="12843-135">: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* `[Url]`<span data-ttu-id="12843-136">: valida que la propiedad tenga un formato de URL.</span><span class="sxs-lookup"><span data-stu-id="12843-136">: Validates that the property has a URL format.</span></span>
* `[Remote]`<span data-ttu-id="12843-137">: valida la entrada en el cliente mediante una llamada a un método de acción en el servidor.</span><span class="sxs-lookup"><span data-stu-id="12843-137">: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="12843-138">Vea [Atributo [Remote]](#remote-attribute) para obtener más información sobre el comportamiento de este atributo.</span><span class="sxs-lookup"><span data-stu-id="12843-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="12843-139">En el espacio de nombres [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) encontrará una lista completa de atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="12843-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="12843-140">Mensajes de error</span><span class="sxs-lookup"><span data-stu-id="12843-140">Error messages</span></span>

<span data-ttu-id="12843-141">Los atributos de validación permiten especificar el mensaje de error que se mostrará para una entrada no válida.</span><span class="sxs-lookup"><span data-stu-id="12843-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="12843-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12843-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="12843-143">Internamente, los atributos llaman a `String.Format` con un marcador de posición para el nombre de campo y, en ocasiones, marcadores de posición adicionales.</span><span class="sxs-lookup"><span data-stu-id="12843-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="12843-144">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12843-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="12843-145">Cuando se aplica a una propiedad `Name`, el mensaje de error creado por el código anterior sería "La longitud del nombre debe estar entre 6 y 8".</span><span class="sxs-lookup"><span data-stu-id="12843-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="12843-146">Para averiguar qué parámetros se pasan a `String.Format` para el mensaje de error de un atributo determinado, vea el [código fuente de DataAnnotations](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span><span class="sxs-lookup"><span data-stu-id="12843-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="12843-147">Atributo [Required]</span><span class="sxs-lookup"><span data-stu-id="12843-147">[Required] attribute</span></span>

<span data-ttu-id="12843-148">De forma predeterminada, el sistema de validación trata las propiedades o los parámetros que no aceptan valores NULL como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="12843-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="12843-149">Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) como `decimal` y `int` no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="12843-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="12843-150">Validación de [Required] en el servidor</span><span class="sxs-lookup"><span data-stu-id="12843-150">[Required] validation on the server</span></span>

<span data-ttu-id="12843-151">En el servidor, si la propiedad es NULL, se considera que falta un valor requerido.</span><span class="sxs-lookup"><span data-stu-id="12843-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="12843-152">Un campo que no acepta valores NULL siempre es válido, y el mensaje de error del atributo [Required] no se muestra nunca.</span><span class="sxs-lookup"><span data-stu-id="12843-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="12843-153">Aun así, el enlace de modelos para una propiedad que no acepta valores NULL podría fallar, lo que genera un mensaje de error como `The value '' is invalid`.</span><span class="sxs-lookup"><span data-stu-id="12843-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="12843-154">Para especificar un mensaje de error personalizado para la validación del lado servidor de tipos que no aceptan valores NULL, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="12843-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="12843-155">Haga que el campo acepte valores NULL (por ejemplo, `decimal?` en lugar de `decimal`).</span><span class="sxs-lookup"><span data-stu-id="12843-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="12843-156">Los tipos de valor [Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) se tratan como tipos estándar que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="12843-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="12843-157">Especifique el mensaje de error predeterminado que el enlace de modelos va a usar, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="12843-158">Para obtener más información sobre los errores de enlace de modelos para los que se pueden establecer mensajes predeterminados, vea <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span><span class="sxs-lookup"><span data-stu-id="12843-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="12843-159">Validación de [Required] en el cliente</span><span class="sxs-lookup"><span data-stu-id="12843-159">[Required] validation on the client</span></span>

<span data-ttu-id="12843-160">Las cadenas y los tipos que no aceptan valores NULL se tratan de forma diferente en el cliente, en comparación con el servidor.</span><span class="sxs-lookup"><span data-stu-id="12843-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="12843-161">En el cliente:</span><span class="sxs-lookup"><span data-stu-id="12843-161">On the client:</span></span>

* <span data-ttu-id="12843-162">Un valor se considera presente solo si se especifica una entrada para él.</span><span class="sxs-lookup"><span data-stu-id="12843-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="12843-163">Por lo tanto, la validación del lado cliente controla los tipos que no aceptan valores NULL del mismo modo que los tipos que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="12843-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="12843-164">El método [required](https://jqueryvalidation.org/required-method/) de jQuery Validate considera que un espacio en blanco en un campo de cadena es una entrada válida.</span><span class="sxs-lookup"><span data-stu-id="12843-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="12843-165">La validación del lado servidor considera que un campo de cadena necesario no es válido si solo se especifica un espacio en blanco.</span><span class="sxs-lookup"><span data-stu-id="12843-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="12843-166">Como se indicó anteriormente, los tipos que no aceptan valores NULL se tratan como si tuvieran un atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="12843-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="12843-167">Esto significa que se obtiene la validación del lado cliente incluso si no se aplica el atributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="12843-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="12843-168">Si no usa el atributo, aparecerá un mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="12843-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="12843-169">Para especificar un mensaje de error personalizado, use el atributo.</span><span class="sxs-lookup"><span data-stu-id="12843-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="12843-170">Atributo [Remote]</span><span class="sxs-lookup"><span data-stu-id="12843-170">[Remote] attribute</span></span>

<span data-ttu-id="12843-171">El atributo `[Remote]` implementa la validación del lado cliente que requiere llamar a un método en el servidor para determinar si la entrada del campo es válida.</span><span class="sxs-lookup"><span data-stu-id="12843-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="12843-172">Por ejemplo, la aplicación podría tener que comprobar si un nombre de usuario ya está en uso.</span><span class="sxs-lookup"><span data-stu-id="12843-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="12843-173">Para implementar la validación remota:</span><span class="sxs-lookup"><span data-stu-id="12843-173">To implement remote validation:</span></span>

1. <span data-ttu-id="12843-174">Cree un método de acción para que lo llame JavaScript.</span><span class="sxs-lookup"><span data-stu-id="12843-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="12843-175">El método [remote](https://jqueryvalidation.org/remote-method/) de jQuery Validate espera una respuesta JSON:</span><span class="sxs-lookup"><span data-stu-id="12843-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * `"true"` <span data-ttu-id="12843-176">significa que los datos de entrada son válidos.</span><span class="sxs-lookup"><span data-stu-id="12843-176">means the input data is valid.</span></span>
   * `"false"`<span data-ttu-id="12843-177">, `undefined` o `null` significan que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="12843-177">, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="12843-178">Muestre el mensaje de error predeterminado.</span><span class="sxs-lookup"><span data-stu-id="12843-178">Display the default error message.</span></span>
   * <span data-ttu-id="12843-179">Cualquier otra cadena significa que la entrada no es válida.</span><span class="sxs-lookup"><span data-stu-id="12843-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="12843-180">Muestre la cadena como un mensaje de error personalizado.</span><span class="sxs-lookup"><span data-stu-id="12843-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="12843-181">A continuación encontrará un ejemplo de un método de acción que devuelve un mensaje de error personalizado:</span><span class="sxs-lookup"><span data-stu-id="12843-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="12843-182">En la clase de modelo, anote la propiedad con un atributo `[Remote]` que apunte al método de acción de validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]

### <a name="additional-fields"></a><span data-ttu-id="12843-183">Campos adicionales</span><span class="sxs-lookup"><span data-stu-id="12843-183">Additional fields</span></span>

<span data-ttu-id="12843-184">La propiedad `AdditionalFields` del atributo `[Remote]` permite validar combinaciones de campos con los datos del servidor.</span><span class="sxs-lookup"><span data-stu-id="12843-184">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="12843-185">Por ejemplo, si el modelo `User` tuviera las propiedades `FirstName` y `LastName`, podría interesarle comprobar que ningún usuario actual tuviera ya ese par de nombres.</span><span class="sxs-lookup"><span data-stu-id="12843-185">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="12843-186">En el siguiente ejemplo se muestra cómo usar `AdditionalFields`:</span><span class="sxs-lookup"><span data-stu-id="12843-186">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

`AdditionalFields` <span data-ttu-id="12843-187">podría configurarse explícitamente para las cadenas `"FirstName"` y `"LastName"`, pero, al usar el operador [`nameof`](/dotnet/csharp/language-reference/keywords/nameof), se simplifica la refactorización posterior.</span><span class="sxs-lookup"><span data-stu-id="12843-187">could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="12843-188">El método de acción para esta validación debe aceptar los argumentos de nombre y apellido:</span><span class="sxs-lookup"><span data-stu-id="12843-188">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="12843-189">Cuando el usuario escribe un nombre o un apellido, JavaScript realiza una llamada remota para comprobar si ese par de nombres ya existe.</span><span class="sxs-lookup"><span data-stu-id="12843-189">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="12843-190">Para validar dos o más campos adicionales, proporciónelos como una lista delimitada por comas.</span><span class="sxs-lookup"><span data-stu-id="12843-190">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="12843-191">Por ejemplo, para agregar una propiedad `MiddleName` al modelo, establezca el atributo `[Remote]` tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-191">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

`AdditionalFields`<span data-ttu-id="12843-192">, al igual que todos los argumentos de atributo, debe ser una expresión constante.</span><span class="sxs-lookup"><span data-stu-id="12843-192">, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="12843-193">Por lo tanto, no use una [cadena interpolada](/dotnet/csharp/language-reference/keywords/interpolated-strings) ni llame a <xref:System.String.Join*> para inicializar `AdditionalFields`.</span><span class="sxs-lookup"><span data-stu-id="12843-193">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="12843-194">Alternativas a los atributos integrados</span><span class="sxs-lookup"><span data-stu-id="12843-194">Alternatives to built-in attributes</span></span>

<span data-ttu-id="12843-195">Si necesita una validación que no proporcionan los atributos integrados, puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-195">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="12843-196">[Crear atributos personalizados](#custom-attributes)</span><span class="sxs-lookup"><span data-stu-id="12843-196">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="12843-197">[Implementar IValidatableObject](#ivalidatableobject)</span><span class="sxs-lookup"><span data-stu-id="12843-197">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="12843-198">Atributos personalizados</span><span class="sxs-lookup"><span data-stu-id="12843-198">Custom attributes</span></span>

<span data-ttu-id="12843-199">Para los escenarios que no se controlan mediante los atributos de validación integrados, puede crear atributos de validación personalizados.</span><span class="sxs-lookup"><span data-stu-id="12843-199">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="12843-200">Cree una clase que herede de <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> y reemplace el método <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*>.</span><span class="sxs-lookup"><span data-stu-id="12843-200">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="12843-201">El método `IsValid` acepta un objeto denominado *value*, que es la entrada que se va a validar.</span><span class="sxs-lookup"><span data-stu-id="12843-201">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="12843-202">Una sobrecarga también acepta un objeto `ValidationContext`, que proporciona información adicional, como la instancia del modelo creada por el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="12843-202">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="12843-203">El siguiente ejemplo valida que la fecha de lanzamiento de una película del género *Classic* no sea posterior a un año especificado.</span><span class="sxs-lookup"><span data-stu-id="12843-203">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="12843-204">El atributo `[ClassicMovie2]` comprueba primero el género y continúa únicamente si es *Classic*.</span><span class="sxs-lookup"><span data-stu-id="12843-204">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="12843-205">Para las películas que se identifican como clásicos, comprueba la fecha de lanzamiento a fin de asegurarse de que no sea posterior al límite que se ha pasado al constructor de atributo.</span><span class="sxs-lookup"><span data-stu-id="12843-205">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="12843-206">La variable `movie` del ejemplo anterior representa un objeto `Movie` que contiene los datos del envío del formulario.</span><span class="sxs-lookup"><span data-stu-id="12843-206">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="12843-207">El método `IsValid` comprueba la fecha y el género.</span><span class="sxs-lookup"><span data-stu-id="12843-207">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="12843-208">Si la validación es correcta, `IsValid` devuelve un código `ValidationResult.Success`.</span><span class="sxs-lookup"><span data-stu-id="12843-208">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="12843-209">Si se produce un error de validación, se devuelve un `ValidationResult` con un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="12843-209">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="12843-210">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="12843-210">IValidatableObject</span></span>

<span data-ttu-id="12843-211">El ejemplo anterior solo funciona con tipos `Movie`.</span><span class="sxs-lookup"><span data-stu-id="12843-211">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="12843-212">Otra opción para la validación del nivel de clase consiste en implementar `IValidatableObject` en la clase de modelo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-212">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="12843-213">Validación de nodo de nivel superior</span><span class="sxs-lookup"><span data-stu-id="12843-213">Top-level node validation</span></span>

<span data-ttu-id="12843-214">Los nodos de nivel superior incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-214">Top-level nodes include:</span></span>

* <span data-ttu-id="12843-215">Parámetros de acción</span><span class="sxs-lookup"><span data-stu-id="12843-215">Action parameters</span></span>
* <span data-ttu-id="12843-216">Propiedades de controlador</span><span class="sxs-lookup"><span data-stu-id="12843-216">Controller properties</span></span>
* <span data-ttu-id="12843-217">Parámetros de controlador de página</span><span class="sxs-lookup"><span data-stu-id="12843-217">Page handler parameters</span></span>
* <span data-ttu-id="12843-218">Propiedades de modelo de página</span><span class="sxs-lookup"><span data-stu-id="12843-218">Page model properties</span></span>

<span data-ttu-id="12843-219">Los nodos de nivel superior enlazados al modelo se validan además de la validación de las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="12843-219">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="12843-220">En el ejemplo siguiente de la aplicación de muestra, el método `VerifyPhone` usa <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> para validar el parámetro de acción `phone`:</span><span class="sxs-lookup"><span data-stu-id="12843-220">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="12843-221">Los nodos de nivel superior pueden usar <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> con atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="12843-221">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="12843-222">En el ejemplo siguiente de la aplicación de muestra, el método `CheckAge` especifica que el parámetro `age` debe estar enlazado desde la cadena de consulta al enviar el formulario:</span><span class="sxs-lookup"><span data-stu-id="12843-222">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="12843-223">En la página Comprobar edad (*CheckAge.cshtml*), hay dos formularios.</span><span class="sxs-lookup"><span data-stu-id="12843-223">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="12843-224">El primer formulario envía un valor `Age` de `99` como una cadena de consulta: `https://localhost:5001/Users/CheckAge?Age=99`.</span><span class="sxs-lookup"><span data-stu-id="12843-224">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="12843-225">Al enviar un parámetro `age` con un formato correcto desde la cadena de consulta, el formulario se valida.</span><span class="sxs-lookup"><span data-stu-id="12843-225">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="12843-226">El segundo formulario de la página Comprobar edad envía el valor `Age` en el cuerpo de la solicitud, y se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="12843-226">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="12843-227">Se produce un error en el enlace porque el parámetro `age` debe provenir de una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="12843-227">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="12843-228">Cuando se ejecuta con `CompatibilityVersion.Version_2_1` o versiones posteriores, la validación de nodo de nivel superior está habilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="12843-228">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="12843-229">En caso contrario, la validación del nodo de nivel superior está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="12843-229">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="12843-230">La opción predeterminada se puede invalidar si se establece la propiedad <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> en (`Startup.ConfigureServices`), como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="12843-230">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="12843-231">Número máximo de errores</span><span class="sxs-lookup"><span data-stu-id="12843-231">Maximum errors</span></span>

<span data-ttu-id="12843-232">La validación se detiene cuando se alcanza el número máximo de errores (200 de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="12843-232">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="12843-233">Puede configurar este número con el siguiente código en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12843-233">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="12843-234">Recursividad máxima</span><span class="sxs-lookup"><span data-stu-id="12843-234">Maximum recursion</span></span>

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> <span data-ttu-id="12843-235">recorre el gráfico de objetos del modelo que se está validando.</span><span class="sxs-lookup"><span data-stu-id="12843-235">traverses the object graph of the model being validated.</span></span> <span data-ttu-id="12843-236">En el caso de los modelos muy profundos o infinitamente recursivos, la validación podría causar un desbordamiento de pila.</span><span class="sxs-lookup"><span data-stu-id="12843-236">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="12843-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) proporciona una manera de detener pronto la validación si la recursividad del visitante supera la profundidad configurada.</span><span class="sxs-lookup"><span data-stu-id="12843-237">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="12843-238">El valor predeterminado de `MvcOptions.MaxValidationDepth` es 200 cuando se ejecuta con `CompatibilityVersion.Version_2_2` o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="12843-238">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="12843-239">Para las versiones anteriores, el valor es NULL, lo que significa que no hay restricción de profundidad.</span><span class="sxs-lookup"><span data-stu-id="12843-239">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="12843-240">Cortocircuito automático</span><span class="sxs-lookup"><span data-stu-id="12843-240">Automatic short-circuit</span></span>

<span data-ttu-id="12843-241">La validación cortocircuita (se omite) automáticamente si el gráfico de modelo no requiere validación.</span><span class="sxs-lookup"><span data-stu-id="12843-241">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="12843-242">Entre los objetos para los que el tiempo de ejecución omite la validación se incluyen las colecciones de elementos primitivos (como `byte[]`, `string[]` y `Dictionary<string, string>`) y gráficos de objeto complejo que no tienen los validadores.</span><span class="sxs-lookup"><span data-stu-id="12843-242">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="12843-243">Deshabilitación de la validación</span><span class="sxs-lookup"><span data-stu-id="12843-243">Disable validation</span></span>

<span data-ttu-id="12843-244">Para deshabilitar la validación:</span><span class="sxs-lookup"><span data-stu-id="12843-244">To disable validation:</span></span>

1. <span data-ttu-id="12843-245">Cree una implementación de `IObjectModelValidator` que no marque ningún campo como no válido.</span><span class="sxs-lookup"><span data-stu-id="12843-245">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="12843-246">Agregue el código siguiente a `Startup.ConfigureServices` para reemplazar la implementación predeterminada de `IObjectModelValidator` en el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="12843-246">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="12843-247">Es posible que todavía vea errores de estado del modelo originados en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="12843-247">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="12843-248">Validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="12843-248">Client-side validation</span></span>

<span data-ttu-id="12843-249">La validación del lado cliente impide realizar el envío hasta que el formulario sea válido.</span><span class="sxs-lookup"><span data-stu-id="12843-249">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="12843-250">El botón Enviar ejecuta JavaScript para enviar el formulario o mostrar mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="12843-250">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="12843-251">La validación del lado cliente evita un recorrido de ida y vuelta innecesario en el servidor cuando hay errores de entrada en un formulario.</span><span class="sxs-lookup"><span data-stu-id="12843-251">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="12843-252">Las siguientes referencias de script de *_Layout.cshtml* y *_ValidationScriptsPartial.cshtml* admiten la validación del lado cliente:</span><span class="sxs-lookup"><span data-stu-id="12843-252">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="12843-253">El script [Validación discreta de jQuery](https://github.com/aspnet/jquery-validation-unobtrusive) es una biblioteca front-end personalizada de Microsoft que se basa en el conocido complemento [Validación de jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="12843-253">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="12843-254">Si no usa Validación discreta de jQuery, deberá codificar la misma lógica de validación dos veces: una vez en los atributos de validación del lado servidor en las propiedades del modelo y luego en los scripts del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="12843-254">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="12843-255">En su lugar, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan los atributos de validación y escriben metadatos de las propiedades del modelo para representar atributos `data-` HTML 5 para los elementos de formulario que necesitan validación.</span><span class="sxs-lookup"><span data-stu-id="12843-255">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="12843-256">Validación discreta de jQuery analiza los atributos `data-` y pasa la lógica a jQuery Validate. De este modo, la lógica de validación del lado servidor se "copia" de manera eficaz en el cliente.</span><span class="sxs-lookup"><span data-stu-id="12843-256">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="12843-257">Puede mostrar errores de validación en el cliente mediante el uso de asistentes de etiquetas, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="12843-257">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="12843-258">Los anteriores asistentes de etiquetas representan el siguiente código HTML.</span><span class="sxs-lookup"><span data-stu-id="12843-258">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="12843-259">Tenga en cuenta que los atributos `data-` en los resultados HTML corresponden a los atributos de validación para la propiedad `ReleaseDate`.</span><span class="sxs-lookup"><span data-stu-id="12843-259">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="12843-260">El atributo `data-val-required` contiene un mensaje de error que se muestra si el usuario no rellena el campo de fecha de estreno.</span><span class="sxs-lookup"><span data-stu-id="12843-260">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="12843-261">La Validación discreta de jQuery pasa este valor al método [`required()`](https://jqueryvalidation.org/required-method/) de la Validación de jQuery, que muestra un mensaje en el elemento **\<span>** que lo acompaña.</span><span class="sxs-lookup"><span data-stu-id="12843-261">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="12843-262">La validación del tipo de datos se basa en el tipo .NET de una propiedad, a menos que lo reemplace un atributo `[DataType]`.</span><span class="sxs-lookup"><span data-stu-id="12843-262">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="12843-263">Los exploradores tienen sus propios mensajes de error de predeterminados, pero el paquete de Validación discreta de jQuery Validate puede invalidar esos mensajes.</span><span class="sxs-lookup"><span data-stu-id="12843-263">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> `[DataType]` <span data-ttu-id="12843-264">Los atributos y las subclases `[EmailAddress]`, como , permiten especificar el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="12843-264">attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="12843-265">Agregar validación a formularios dinámicos</span><span class="sxs-lookup"><span data-stu-id="12843-265">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="12843-266">Validación discreta de jQuery pasa los parámetros y la lógica de validación a jQuery Validate cuando la página se carga por primera vez.</span><span class="sxs-lookup"><span data-stu-id="12843-266">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="12843-267">Por lo tanto, la validación no funciona automáticamente en los formularios generados dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="12843-267">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="12843-268">Para habilitar la validación, hay que indicarle a Validación discreta de jQuery que analice el formulario dinámico inmediatamente después de su creación.</span><span class="sxs-lookup"><span data-stu-id="12843-268">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="12843-269">Por ejemplo, en el código siguiente se configura la validación del lado cliente en un formulario agregado mediante AJAX.</span><span class="sxs-lookup"><span data-stu-id="12843-269">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="12843-270">El método `$.validator.unobtrusive.parse()` acepta un selector de jQuery para su único argumento.</span><span class="sxs-lookup"><span data-stu-id="12843-270">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="12843-271">Este método indica a Validación discreta de jQuery que analice los atributos `data-` de formularios dentro de ese selector.</span><span class="sxs-lookup"><span data-stu-id="12843-271">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="12843-272">Después, los valores de estos atributos se pasan al complemento de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="12843-272">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="12843-273">Agregar validación a controles dinámicos</span><span class="sxs-lookup"><span data-stu-id="12843-273">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="12843-274">El método `$.validator.unobtrusive.parse()` funciona en todo el formulario, no en los controles individuales generados dinámicamente, como `<input>` y `<select/>`.</span><span class="sxs-lookup"><span data-stu-id="12843-274">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="12843-275">Para volver a analizar el formulario, quite los datos de validación que se agregaron cuando el formulario se analizó anteriormente, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-275">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="12843-276">Validación del lado cliente personalizada</span><span class="sxs-lookup"><span data-stu-id="12843-276">Custom client-side validation</span></span>

<span data-ttu-id="12843-277">Para personalizar la validación del lado cliente, es necesario generar atributos HTML `data-` que funcionen con un adaptador personalizado de jQuery Validate.</span><span class="sxs-lookup"><span data-stu-id="12843-277">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="12843-278">El siguiente ejemplo de código de adaptador se escribió para los atributos `ClassicMovie` y `ClassicMovie2` que se introdujeron anteriormente en este artículo:</span><span class="sxs-lookup"><span data-stu-id="12843-278">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="12843-279">Para obtener información sobre cómo escribir adaptadores, vea la [documentación de jQuery Validate](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="12843-279">For information about how to write adapters, see the [jQuery Validate documentation](http://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="12843-280">El uso de un adaptador para un campo determinado se desencadena mediante atributos `data-` que:</span><span class="sxs-lookup"><span data-stu-id="12843-280">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="12843-281">Marcan que el campo está sujeto a validación (`data-val="true"`).</span><span class="sxs-lookup"><span data-stu-id="12843-281">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="12843-282">Identifican un nombre de regla de validación y un texto de mensaje de error (por ejemplo, `data-val-rulename="Error message."`).</span><span class="sxs-lookup"><span data-stu-id="12843-282">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="12843-283">Proporcionan los parámetros adicionales que necesite el validador (por ejemplo, `data-val-rulename-parm1="value"`).</span><span class="sxs-lookup"><span data-stu-id="12843-283">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="12843-284">En el ejemplo siguiente se muestran los atributos `data-` para el atributo `ClassicMovie` de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12843-284">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="12843-285">Como se indicó anteriormente, los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los [asistentes de HTML](xref:mvc/views/overview) usan información procedente de los atributos de validación para representar atributos `data-`.</span><span class="sxs-lookup"><span data-stu-id="12843-285">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="12843-286">Hay dos opciones para escribir código que dé como resultado la creación de atributos HTML `data-` personalizados:</span><span class="sxs-lookup"><span data-stu-id="12843-286">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="12843-287">Puede crear una clase que derive de `AttributeAdapterBase<TAttribute>` y una clase que implemente `IValidationAttributeAdapterProvider` y, después, registrar el atributo y su adaptador en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="12843-287">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="12843-288">Este método sigue el [principio de responsabilidad única](https://wikipedia.org/wiki/Single_responsibility_principle), ya que el código de validación relacionado con servidor y el relacionado con el cliente se encuentran en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="12843-288">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="12843-289">El adaptador también cuenta con una ventaja: debido a que está registrado en la inserción de dependencias, tiene a su disposición otros servicios de la inserción de dependencias si es necesario.</span><span class="sxs-lookup"><span data-stu-id="12843-289">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="12843-290">Puede implementar `IClientModelValidator` en la clase `ValidationAttribute`.</span><span class="sxs-lookup"><span data-stu-id="12843-290">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="12843-291">Este método es adecuado si el atributo no realiza ninguna validación en el lado servidor y no necesita ningún servicio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="12843-291">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="12843-292">AttributeAdapter para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="12843-292">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="12843-293">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovie` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="12843-293">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="12843-294">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="12843-294">To add client validation by using this method:</span></span>

1. <span data-ttu-id="12843-295">Cree una clase de adaptador de atributo para el atributo de validación personalizado.</span><span class="sxs-lookup"><span data-stu-id="12843-295">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="12843-296">Derive la clase de [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="12843-296">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="12843-297">Cree un método `AddValidation` que agregue atributos `data-` a la salida representada, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12843-297">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="12843-298">Cree una clase de proveedor de adaptador que implemente <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span><span class="sxs-lookup"><span data-stu-id="12843-298">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="12843-299">En el método `GetAttributeAdapter`, pase el atributo personalizado al constructor del adaptador, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12843-299">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="12843-300">Registre el proveedor del adaptador para la inserción de dependencias en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12843-300">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="12843-301">IClientModelValidator para la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="12843-301">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="12843-302">Este método para representar atributos `data-` en HTML es lo que usa el atributo `ClassicMovie2` en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="12843-302">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="12843-303">Para agregar la validación de cliente mediante este método:</span><span class="sxs-lookup"><span data-stu-id="12843-303">To add client validation by using this method:</span></span>

* <span data-ttu-id="12843-304">En el atributo de validación personalizado, implemente la interfaz `IClientModelValidator` y cree un método `AddValidation`.</span><span class="sxs-lookup"><span data-stu-id="12843-304">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="12843-305">En el método `AddValidation`, agregue atributos `data-` para la validación, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12843-305">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="12843-306">Deshabilitación de la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="12843-306">Disable client-side validation</span></span>

<span data-ttu-id="12843-307">El código siguiente deshabilita la validación de cliente en las vistas de MVC:</span><span class="sxs-lookup"><span data-stu-id="12843-307">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="12843-308">Esto solo funciona en las vistas de MVC, no en Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="12843-308">This works only in MVC views, not in Razor Pages.</span></span> <span data-ttu-id="12843-309">Otra opción para deshabilitar la validación de cliente consiste en marcar como comentario la referencia a `_ValidationScriptsPartial` en el archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="12843-309">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12843-310">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="12843-310">Additional resources</span></span>

* [<span data-ttu-id="12843-311">Espacio de nombres System.ComponentModel.DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="12843-311">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="12843-312">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="12843-312">Model Binding</span></span>](model-binding.md)
