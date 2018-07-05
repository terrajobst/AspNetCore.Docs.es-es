---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validar la entrada del usuario en ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: tfitzmac
description: Este artículo describe cómo validar la información que se obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escriban válido información en HTML forms en un sistema autónomo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 2a35e9895c5b711d5c6c5544987f99fe7e2e0085
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368232"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="41668-103">Validar la entrada del usuario en los sitios de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="41668-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="41668-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="41668-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="41668-105">Este artículo describe cómo validar la información que se obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escriban válido información en HTML forms en un sitio de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="41668-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="41668-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="41668-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="41668-107">Cómo comprobar que una entrada del usuario coincide con los criterios de validación que se definen.</span><span class="sxs-lookup"><span data-stu-id="41668-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="41668-108">Cómo determinar si se han pasado todas las pruebas de validación.</span><span class="sxs-lookup"><span data-stu-id="41668-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="41668-109">Cómo mostrar errores de validación (y cómo darles formato).</span><span class="sxs-lookup"><span data-stu-id="41668-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="41668-110">Cómo validar los datos que no proceden directamente de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="41668-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="41668-111">Estos son los conceptos presentados en el artículo de programación de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="41668-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="41668-112">El `Validation` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="41668-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="41668-113">El `Html.ValidationSummary` y `Html.ValidationMessage` métodos.</span><span class="sxs-lookup"><span data-stu-id="41668-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="41668-114">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="41668-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="41668-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="41668-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="41668-116">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="41668-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="41668-117">Este artículo contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="41668-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="41668-118">Información general sobre la validación de entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="41668-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="41668-119">Validar la entrada del usuario</span><span class="sxs-lookup"><span data-stu-id="41668-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="41668-120">Agregar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="41668-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="41668-121">Errores de validación de formato</span><span class="sxs-lookup"><span data-stu-id="41668-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="41668-122">Validación de datos que no proceden directamente de los usuarios</span><span class="sxs-lookup"><span data-stu-id="41668-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="41668-123">Información general sobre la validación de entrada de usuario</span><span class="sxs-lookup"><span data-stu-id="41668-123">Overview of User Input Validation</span></span>

<span data-ttu-id="41668-124">Si preguntar a los usuarios escribir información en una página, por ejemplo, en un formulario, es importante asegurarse de que son válidos los valores que escriba.</span><span class="sxs-lookup"><span data-stu-id="41668-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="41668-125">Por ejemplo, no desea que procese un formulario que le falta información crítica.</span><span class="sxs-lookup"><span data-stu-id="41668-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="41668-126">Cuando los usuarios escriben valores en un formulario HTML, los valores que escriba son cadenas.</span><span class="sxs-lookup"><span data-stu-id="41668-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="41668-127">En muchos casos, los valores que necesita son otros tipos de datos, como números enteros o fechas.</span><span class="sxs-lookup"><span data-stu-id="41668-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="41668-128">Por lo tanto, también debe asegurarse de que los valores que especifican los usuarios se pueden convertir correctamente a los tipos de datos adecuado.</span><span class="sxs-lookup"><span data-stu-id="41668-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="41668-129">Es posible que también tiene ciertas restricciones en los valores.</span><span class="sxs-lookup"><span data-stu-id="41668-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="41668-130">Incluso si los usuarios escriben correctamente un entero, por ejemplo, es posible que deberá asegurarse de que el valor se encuentra dentro de un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="41668-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Errores de validación que utilizan las clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="41668-132">**Importante** también es importante para la seguridad de validar la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="41668-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="41668-133">Al restringir los valores que los usuarios pueden escribir en los formularios, reducir la posibilidad de que un usuario puede escribir un valor que puede poner en peligro la seguridad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="41668-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="41668-134">Validar la entrada del usuario</span><span class="sxs-lookup"><span data-stu-id="41668-134">Validating User Input</span></span>

<span data-ttu-id="41668-135">En ASP.NET Web Pages 2, puede usar el `Validator` auxiliares para probar la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="41668-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="41668-136">El enfoque básico es hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="41668-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="41668-137">Determinar qué entrada elementos (campos) que se va a validar.</span><span class="sxs-lookup"><span data-stu-id="41668-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="41668-138">Normalmente se validan los valores de `<input>` elementos en un formulario.</span><span class="sxs-lookup"><span data-stu-id="41668-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="41668-139">Sin embargo, es una buena práctica para validar todas las entradas, incluso de entrada procede de un elemento restringido como un `<select>` lista.</span><span class="sxs-lookup"><span data-stu-id="41668-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="41668-140">Esto ayuda a asegurarse de que los usuarios no omitir los controles en una página y enviar un formulario.</span><span class="sxs-lookup"><span data-stu-id="41668-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="41668-141">En el código de la página Agregar comprobaciones de validación individuales para cada elemento de entrada mediante el uso de métodos de la `Validation` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="41668-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="41668-142">Para comprobar los campos obligatorios, use `Validation.RequireField(field, [error message])` (para un campo individual) o `Validation.RequireFields(field1, field2, ...))` (para obtener una lista de campos).</span><span class="sxs-lookup"><span data-stu-id="41668-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="41668-143">Para otros tipos de validación, utilice `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="41668-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="41668-144">Para `ValidationType`, puede usar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="41668-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="41668-145">Cuando se envía la página, compruebe si ha pasado la validación mediante la comprobación de `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="41668-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="41668-146">Si hay errores de validación, omitir el procesamiento normal de página.</span><span class="sxs-lookup"><span data-stu-id="41668-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="41668-147">Por ejemplo, si es el propósito de la página Actualizar una base de datos, no hacerlo hasta que se han corregido todos los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="41668-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="41668-148">Si hay errores de validación, mostrar mensajes de error en el marcado de la página mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o ambos.</span><span class="sxs-lookup"><span data-stu-id="41668-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="41668-149">El ejemplo siguiente muestra una página que muestra estos pasos.</span><span class="sxs-lookup"><span data-stu-id="41668-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="41668-150">Para ver cómo funciona la validación, ejecute esta página y deliberadamente cometen errores.</span><span class="sxs-lookup"><span data-stu-id="41668-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="41668-151">Por ejemplo, aquí es la página de aspecto si se olvida de escribir un nombre del curso, si escribe un, y si escribe una fecha no válida:</span><span class="sxs-lookup"><span data-stu-id="41668-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Errores de validación en la página representada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="41668-153">Agregar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="41668-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="41668-154">De forma predeterminada, se validan proporcionados por el usuario después de que los usuarios enviar la página, es decir, la validación se realiza en el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="41668-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="41668-155">Una desventaja de este enfoque es que los usuarios no saben que ha realizado un error hasta después de envíe la página.</span><span class="sxs-lookup"><span data-stu-id="41668-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="41668-156">Si un formulario es larga o compleja, informan de errores solo una vez enviada la página puede ser un problema al usuario.</span><span class="sxs-lookup"><span data-stu-id="41668-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="41668-157">Puede agregar compatibilidad para realizar la validación en el script de cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="41668-158">En ese caso, la validación se realiza conforme los usuarios trabajan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="41668-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="41668-159">Por ejemplo, imagine que especifica que un valor debe ser un entero.</span><span class="sxs-lookup"><span data-stu-id="41668-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="41668-160">Si un usuario escribe un valor no entero, se notifica el error tan pronto como el usuario deja el campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="41668-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="41668-161">Los usuarios reciben una respuesta inmediata, que es conveniente para ellos.</span><span class="sxs-lookup"><span data-stu-id="41668-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="41668-162">Validación basada en cliente también puede reducir el número de veces que el usuario tiene que enviar el formulario para corregir los errores varios.</span><span class="sxs-lookup"><span data-stu-id="41668-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="41668-163">Incluso si usa la validación del lado cliente, siempre también se realiza la validación en el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="41668-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="41668-164">Realizar la validación en el código de servidor es una medida de seguridad, en caso de que los usuarios omiten la validación basada en cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="41668-165">Registrar las siguientes bibliotecas de JavaScript en la página:</span><span class="sxs-lookup"><span data-stu-id="41668-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="41668-166">Dos de las bibliotecas son que se puede cargar desde content delivery network (CDN), por lo que no necesariamente deben tenerlos en su equipo o servidor.</span><span class="sxs-lookup"><span data-stu-id="41668-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="41668-167">Sin embargo, debe tener una copia local de *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="41668-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="41668-168">Si no ya trabaja con una plantilla de WebMatrix (como **Starter Site** ) que incluye la biblioteca, cree un sitio Web Pages que se basa en **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="41668-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="41668-169">A continuación, copie el *.js* archivo al sitio actual.</span><span class="sxs-lookup"><span data-stu-id="41668-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="41668-170">En el marcado, para cada elemento que se está validando, agregue una llamada a `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="41668-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="41668-171">Este método genera atributos que se usan por la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="41668-172">(En lugar de emitir código real de JavaScript, el método emite atributos como `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="41668-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="41668-173">Estos atributos admiten validación del cliente discreta que usa jQuery para realizar el trabajo).</span><span class="sxs-lookup"><span data-stu-id="41668-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="41668-174">La página siguiente muestra cómo agregar características de validación de cliente en el ejemplo mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="41668-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="41668-175">No todas las comprobaciones de validación se ejecute en el cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="41668-176">En concreto, validación de tipo de datos (entero, fecha etc.) no se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="41668-177">Las comprobaciones siguientes funcionan en el cliente y el servidor:</span><span class="sxs-lookup"><span data-stu-id="41668-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="41668-178">En este ejemplo, no funcionará la prueba para una fecha válida en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="41668-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="41668-179">Sin embargo, la prueba se realizará en el código de servidor.</span><span class="sxs-lookup"><span data-stu-id="41668-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="41668-180">Errores de validación de formato</span><span class="sxs-lookup"><span data-stu-id="41668-180">Formatting Validation Errors</span></span>

<span data-ttu-id="41668-181">Puede controlar cómo se muestran los errores de validación mediante la definición de clases CSS que tienen los nombres reservados siguientes:</span><span class="sxs-lookup"><span data-stu-id="41668-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="41668-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="41668-182">`field-validation-error`.</span></span> <span data-ttu-id="41668-183">Define la salida de la `Html.ValidationMessage` método cuando se está mostrando un error.</span><span class="sxs-lookup"><span data-stu-id="41668-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="41668-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="41668-184">`field-validation-valid`.</span></span> <span data-ttu-id="41668-185">Define la salida de la `Html.ValidationMessage` método cuando no se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="41668-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="41668-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="41668-186">`input-validation-error`.</span></span> <span data-ttu-id="41668-187">Define cómo `<input>` se representan los elementos cuando se produce un error.</span><span class="sxs-lookup"><span data-stu-id="41668-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="41668-188">(Por ejemplo, puede usar esta clase para establecer el color de fondo de un &lt;entrada&gt; elemento a un color diferente si su valor no es válido.) Esta clase CSS que se usa solo durante la validación de cliente (en ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="41668-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="41668-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="41668-189">`input-validation-valid`.</span></span> <span data-ttu-id="41668-190">Define la apariencia de `<input>` elementos cuando no se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="41668-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="41668-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="41668-191">`validation-summary-errors`.</span></span> <span data-ttu-id="41668-192">Define la salida de la `Html.ValidationSummary` método muestra una lista de errores.</span><span class="sxs-lookup"><span data-stu-id="41668-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="41668-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="41668-193">`validation-summary-valid`.</span></span> <span data-ttu-id="41668-194">Define la salida de la `Html.ValidationSummary` método cuando no se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="41668-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="41668-195">La siguiente `<style>` bloque muestra las reglas de las condiciones de error.</span><span class="sxs-lookup"><span data-stu-id="41668-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="41668-196">Si incluye este bloque de estilo en las páginas de ejemplo de anteriormente en este artículo, la presentación de errores se parecerá a la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="41668-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Errores de validación que utilizan las clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="41668-198">Si no usa validación de cliente en ASP.NET Web Pages 2, las clases CSS para el `<input>` elementos (`input-validation-error` y `input-validation-valid` no tendrán ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="41668-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="41668-199">Presentación de errores estáticos y dinámicos</span><span class="sxs-lookup"><span data-stu-id="41668-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="41668-200">Las reglas de CSS vienen en parejas, como `validation-summary-errors` y `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="41668-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="41668-201">Estos pares le permiten definir reglas para las dos condiciones: una condición de error y una condición (sin errores) "normal".</span><span class="sxs-lookup"><span data-stu-id="41668-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="41668-202">Es importante comprender que siempre se representa el marcado para la presentación de errores, incluso si no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="41668-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="41668-203">Por ejemplo, si una página tiene un `Html.ValidationSummary` método en el marcado, el origen de la página contendrá el siguiente marcado incluso cuando se solicita la página por primera vez:</span><span class="sxs-lookup"><span data-stu-id="41668-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="41668-204">En otras palabras, el `Html.ValidationSummary` método siempre representa un `<div>` elemento y una lista, incluso si la lista de errores está vacía.</span><span class="sxs-lookup"><span data-stu-id="41668-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="41668-205">De forma similar, el `Html.ValidationMessage` método siempre representa un `<span>` elemento como un marcador de posición para un error de un campo individual, incluso si no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="41668-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="41668-206">En algunas situaciones, mostrar un mensaje de error, puede hacer que la página redistribuir y puede hacer que los elementos en la página para desplazarse por.</span><span class="sxs-lookup"><span data-stu-id="41668-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="41668-207">Las reglas de CSS que terminan en `-valid` le permiten definir un diseño que puede ayudar a evitar este problema.</span><span class="sxs-lookup"><span data-stu-id="41668-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="41668-208">Por ejemplo, puede definir `field-validation-error` y `field-validation-valid` a ambos haber el mismo tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="41668-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="41668-209">De este modo, el área de presentación para el campo es estático y no cambiará el flujo de la página si se muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="41668-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="41668-210">Validación de datos que no proceden directamente de los usuarios</span><span class="sxs-lookup"><span data-stu-id="41668-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="41668-211">A veces es necesario validar la información que no procede directamente de un formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="41668-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="41668-212">Un ejemplo típico es una página donde se pasa un valor en una cadena de consulta, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="41668-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="41668-213">En este caso, desea asegurarse de que el valor que se pasa a la página (aquí, 1022 para el valor de `classid`) es válido.</span><span class="sxs-lookup"><span data-stu-id="41668-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="41668-214">No puede usar directamente el `Validation` auxiliar para realizar esta validación.</span><span class="sxs-lookup"><span data-stu-id="41668-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="41668-215">Sin embargo, puede usar otras características del sistema de validación, como la capacidad para mostrar mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="41668-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="41668-216">**Importante** validar siempre los valores que obtienen de *cualquier* origen, incluidos los valores de campo de formulario, los valores de cadena de consulta y los valores de cookie.</span><span class="sxs-lookup"><span data-stu-id="41668-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="41668-217">Es fácil para los usuarios cambiar estos valores (quizás para propósitos malintencionados).</span><span class="sxs-lookup"><span data-stu-id="41668-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="41668-218">Por lo que debe comprobar estos valores con el fin de proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="41668-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="41668-219">El ejemplo siguiente muestra cómo se puede validar un valor que se pasa una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="41668-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="41668-220">El código comprueba que el valor no está vacío y que es un entero.</span><span class="sxs-lookup"><span data-stu-id="41668-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="41668-221">Tenga en cuenta que la prueba se realiza cuando la solicitud no es un envío de formulario (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="41668-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="41668-222">Esta prueba pasaría la primera vez que se solicita la página, pero no cuando la solicitud es un envío de formulario.</span><span class="sxs-lookup"><span data-stu-id="41668-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="41668-223">Para mostrar este error, puede agregar el error a la lista de errores de validación mediante una llamada a `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="41668-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="41668-224">Si la página contiene una llamada a la `Html.ValidationSummary` método, el error se muestra aquí, al igual que un error de validación de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="41668-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="41668-225">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="41668-225">Additional Resources</span></span>

[<span data-ttu-id="41668-226">Trabajar con formularios HTML en sitios de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="41668-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
