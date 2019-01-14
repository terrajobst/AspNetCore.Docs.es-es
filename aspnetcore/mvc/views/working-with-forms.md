---
title: Asistentes de etiquetas en formularios de ASP.NET Core
author: rick-anderson
description: En este tema se describen los asistentes de etiquetas que se usan en los formularios.
ms.author: riande
ms.custom: mvc
ms.date: 1/11/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: cd15c641fbf702071bd57510a1d51737f6ab8e19
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099018"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="952fa-103">Asistentes de etiquetas en formularios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="952fa-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="952fa-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette) y [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="952fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="952fa-105">En este documento se explica cómo trabajar con formularios y se detallan los elementos HTML que se usan habitualmente en un formulario.</span><span class="sxs-lookup"><span data-stu-id="952fa-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="952fa-106">El elemento HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) proporciona el mecanismo principal que las aplicaciones web usan a la hora de devolver datos al servidor.</span><span class="sxs-lookup"><span data-stu-id="952fa-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="952fa-107">La mayor parte de este documento se centra en describir los [asistentes de etiquetas](tag-helpers/intro.md) y cómo pueden servir para crear formularios HTML eficaces de manera productiva.</span><span class="sxs-lookup"><span data-stu-id="952fa-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="952fa-108">Se recomienda leer [Introduction to Tag Helpers](tag-helpers/intro.md) (Introducción a los asistentes de etiquetas) antes de este documento.</span><span class="sxs-lookup"><span data-stu-id="952fa-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="952fa-109">En muchos casos, los asistentes de HTML proporcionan un método alternativo para un asistente de etiquetas específico, pero es importante tener en cuenta que los asistentes de etiquetas no reemplazan a los asistentes de HTML y que no hay un asistente de etiquetas para cada asistente de HTML.</span><span class="sxs-lookup"><span data-stu-id="952fa-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="952fa-110">Si existe una alternativa del asistente de HTML, se mencionará aquí.</span><span class="sxs-lookup"><span data-stu-id="952fa-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="952fa-111">Asistente de etiquetas de formulario (Form)</span><span class="sxs-lookup"><span data-stu-id="952fa-111">The Form Tag Helper</span></span>

<span data-ttu-id="952fa-112">El asistente de etiquetas [Form](https://www.w3.org/TR/html401/interact/forms.html) hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="952fa-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="952fa-113">Genera el valor de atributo `action` del elemento HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) de una acción de controlador MVC o ruta con nombre.</span><span class="sxs-lookup"><span data-stu-id="952fa-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="952fa-114">Genera un [token comprobación de solicitudes](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) oculto que impide que se falsifiquen solicitudes entre sitios (cuando se usa con el atributo `[ValidateAntiForgeryToken]` en el método de acción HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="952fa-114">Generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="952fa-115">Proporciona el atributo `asp-route-<Parameter Name>`, donde `<Parameter Name>` se agrega a los valores de ruta.</span><span class="sxs-lookup"><span data-stu-id="952fa-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="952fa-116">Los parámetros `routeValues` de `Html.BeginForm` y `Html.BeginRouteForm` proporcionan una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="952fa-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="952fa-117">Tiene `Html.BeginForm` y `Html.BeginRouteForm` como alternativa del asistente de HTML.</span><span class="sxs-lookup"><span data-stu-id="952fa-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="952fa-118">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="952fa-119">El asistente de etiquetas Form anterior genera el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="952fa-120">El tiempo de ejecución MVC genera el valor de atributo `action` de los atributos del asistente de etiquetas Form `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="952fa-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="952fa-121">El asistente de etiquetas Form genera también un [token de comprobación de solicitudes](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) oculto que impide que se falsifiquen solicitudes entre sitios (cuando se usa con el atributo `[ValidateAntiForgeryToken]` en el método de acción HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="952fa-121">The Form Tag Helper also generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="952fa-122">Proteger un elemento HTML Form puro de la falsificación de solicitudes entre sitios no es tarea fácil, y el asistente de etiquetas Form presta este servicio.</span><span class="sxs-lookup"><span data-stu-id="952fa-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="952fa-123">Uso de una ruta con nombre</span><span class="sxs-lookup"><span data-stu-id="952fa-123">Using a named route</span></span>

<span data-ttu-id="952fa-124">El atributo del asistente de etiquetas `asp-route` puede generar también el marcado del atributo HTML `action`.</span><span class="sxs-lookup"><span data-stu-id="952fa-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="952fa-125">Una aplicación con una [ruta](../../fundamentals/routing.md) denominada `register` podría usar el siguiente marcado para la página de registro:</span><span class="sxs-lookup"><span data-stu-id="952fa-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="952fa-126">Muchas de las vistas de la carpeta *Views/Account* (que se genera cuando se crea una aplicación web con *Cuentas de usuario individuales*) contienen el atributo [asp-route-returnurl](xref:mvc/views/working-with-forms):</span><span class="sxs-lookup"><span data-stu-id="952fa-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="952fa-127">Con las plantillas integradas, `returnUrl` se rellena automáticamente solo cuando alguien intenta obtener acceso a un recurso autorizado, pero no se ha autenticado o no tiene autorización.</span><span class="sxs-lookup"><span data-stu-id="952fa-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="952fa-128">Si se intenta realizar un acceso no autorizado, el middleware de seguridad redirige a la página de inicio de sesión con `returnUrl` configurado.</span><span class="sxs-lookup"><span data-stu-id="952fa-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="952fa-129">Asistente de etiquetas de entrada (Input)</span><span class="sxs-lookup"><span data-stu-id="952fa-129">The Input Tag Helper</span></span>

<span data-ttu-id="952fa-130">El asistente de etiquetas Input enlaza un elemento HTML [\<input&gt;](https://www.w3.org/wiki/HTML/Elements/input) a una expresión de modelo en la vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="952fa-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="952fa-131">Sintaxis:</span><span class="sxs-lookup"><span data-stu-id="952fa-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="952fa-132">El asistente de etiquetas Input hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="952fa-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="952fa-133">Genera los atributos HTML `id` y `name` del nombre de expresión especificado en el atributo `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="952fa-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="952fa-134">`asp-for="Property1.Property2"` es equivalente a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="952fa-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="952fa-135">El nombre de una expresión es lo que se usa para el valor de atributo `asp-for`.</span><span class="sxs-lookup"><span data-stu-id="952fa-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="952fa-136">Vea la sección [Nombres de expresión](#expression-names) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="952fa-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="952fa-137">Establece el valor de atributo HTML `type` según los atributos de tipo de modelo y de [anotación de datos](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) aplicados a la propiedad de modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="952fa-138">No sobrescribirá el valor de atributo HTML `type` cuando se especifique uno.</span><span class="sxs-lookup"><span data-stu-id="952fa-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="952fa-139">Genera atributos de validación [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) a partir de los atributos de [anotación de datos](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) aplicados a las propiedades de modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="952fa-140">Tiene características del asistente de HTML que se superponen a `Html.TextBoxFor` y `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="952fa-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="952fa-141">Vea la sección **Alternativas del asistente de HTML al asistente de etiquetas Input** para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="952fa-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="952fa-142">Permite establecer tipado fuerte.</span><span class="sxs-lookup"><span data-stu-id="952fa-142">Provides strong typing.</span></span> <span data-ttu-id="952fa-143">Si el nombre de la propiedad cambia y no se actualiza el asistente de etiquetas, aparecerá un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="952fa-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="952fa-144">El asistente de etiquetas `Input` establece el atributo HTML `type` en función del tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="952fa-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="952fa-145">En la siguiente tabla se enumeran algunos tipos .NET habituales y el tipo HTML generado correspondiente (no incluimos aquí todos los tipos .NET).</span><span class="sxs-lookup"><span data-stu-id="952fa-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="952fa-146">Tipo de .NET</span><span class="sxs-lookup"><span data-stu-id="952fa-146">.NET type</span></span>|<span data-ttu-id="952fa-147">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="952fa-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="952fa-148">Bool</span><span class="sxs-lookup"><span data-stu-id="952fa-148">Bool</span></span>|<span data-ttu-id="952fa-149">type="checkbox"</span><span class="sxs-lookup"><span data-stu-id="952fa-149">type="checkbox"</span></span>|
|<span data-ttu-id="952fa-150">String</span><span class="sxs-lookup"><span data-stu-id="952fa-150">String</span></span>|<span data-ttu-id="952fa-151">type="text"</span><span class="sxs-lookup"><span data-stu-id="952fa-151">type="text"</span></span>|
|<span data-ttu-id="952fa-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="952fa-152">DateTime</span></span>|<span data-ttu-id="952fa-153">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span><span class="sxs-lookup"><span data-stu-id="952fa-153">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span></span>|
|<span data-ttu-id="952fa-154">Byte</span><span class="sxs-lookup"><span data-stu-id="952fa-154">Byte</span></span>|<span data-ttu-id="952fa-155">type="number"</span><span class="sxs-lookup"><span data-stu-id="952fa-155">type="number"</span></span>|
|<span data-ttu-id="952fa-156">Valor int.</span><span class="sxs-lookup"><span data-stu-id="952fa-156">Int</span></span>|<span data-ttu-id="952fa-157">type="number"</span><span class="sxs-lookup"><span data-stu-id="952fa-157">type="number"</span></span>|
|<span data-ttu-id="952fa-158">Single, Double</span><span class="sxs-lookup"><span data-stu-id="952fa-158">Single, Double</span></span>|<span data-ttu-id="952fa-159">type="number"</span><span class="sxs-lookup"><span data-stu-id="952fa-159">type="number"</span></span>|


<span data-ttu-id="952fa-160">En la siguiente tabla se muestran algunos atributos de [anotación de datos](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) comunes que el asistente de etiquetas Input asignará a tipos de entrada concretos (no incluimos aquí todos los atributo de validación):</span><span class="sxs-lookup"><span data-stu-id="952fa-160">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="952fa-161">Atributo</span><span class="sxs-lookup"><span data-stu-id="952fa-161">Attribute</span></span>|<span data-ttu-id="952fa-162">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="952fa-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="952fa-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="952fa-163">[EmailAddress]</span></span>|<span data-ttu-id="952fa-164">type="email"</span><span class="sxs-lookup"><span data-stu-id="952fa-164">type="email"</span></span>|
|<span data-ttu-id="952fa-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="952fa-165">[Url]</span></span>|<span data-ttu-id="952fa-166">type="url"</span><span class="sxs-lookup"><span data-stu-id="952fa-166">type="url"</span></span>|
|<span data-ttu-id="952fa-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="952fa-167">[HiddenInput]</span></span>|<span data-ttu-id="952fa-168">type="hidden"</span><span class="sxs-lookup"><span data-stu-id="952fa-168">type="hidden"</span></span>|
|<span data-ttu-id="952fa-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="952fa-169">[Phone]</span></span>|<span data-ttu-id="952fa-170">type="tel"</span><span class="sxs-lookup"><span data-stu-id="952fa-170">type="tel"</span></span>|
|<span data-ttu-id="952fa-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="952fa-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="952fa-172">type="password"</span><span class="sxs-lookup"><span data-stu-id="952fa-172">type="password"</span></span>|
|<span data-ttu-id="952fa-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="952fa-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="952fa-174">type="date"</span><span class="sxs-lookup"><span data-stu-id="952fa-174">type="date"</span></span>|
|<span data-ttu-id="952fa-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="952fa-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="952fa-176">type="time"</span><span class="sxs-lookup"><span data-stu-id="952fa-176">type="time"</span></span>|


<span data-ttu-id="952fa-177">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-177">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="952fa-178">El código anterior genera el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="952fa-179">Las anotaciones de datos que se aplican a las propiedades `Email` y `Password` generan metadatos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="952fa-180">El asistente de etiquetas Input usa esos metadatos del modelo y genera atributos [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)`data-val-*`. Vea [Model Validation](../models/validation.md) (Validación del modelo).</span><span class="sxs-lookup"><span data-stu-id="952fa-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="952fa-181">Estos atributos describen los validadores que se van a adjuntar a los campos de entrada,</span><span class="sxs-lookup"><span data-stu-id="952fa-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="952fa-182">lo que proporciona HTML5 discreto y validación de [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="952fa-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="952fa-183">Los atributos discretos tienen el formato `data-val-rule="Error Message"`, donde "rule" es el nombre de la regla de validación (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.). Si aparece un mensaje de error en el atributo, se mostrará como el valor del atributo `data-val-rule`.</span><span class="sxs-lookup"><span data-stu-id="952fa-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="952fa-184">También hay atributos con el formato `data-val-ruleName-argumentName="argumentValue"` que aportan más información sobre la regla, por ejemplo, `data-val-maxlength-max="1024"`.</span><span class="sxs-lookup"><span data-stu-id="952fa-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="952fa-185">Alternativas del asistente de HTML al asistente de etiquetas Input</span><span class="sxs-lookup"><span data-stu-id="952fa-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="952fa-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` y `Html.EditorFor` tienen características que se superponen al asistente de etiquetas Input.</span><span class="sxs-lookup"><span data-stu-id="952fa-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="952fa-187">El asistente de etiquetas Input establecerá automáticamente el atributo `type`, cosa que no ocurrirá con `Html.TextBox` ni `Html.TextBoxFor`.</span><span class="sxs-lookup"><span data-stu-id="952fa-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="952fa-188">`Html.Editor` y `Html.EditorFor` controlan colecciones, objetos complejos y plantillas, pero el asistente de etiquetas Input no.</span><span class="sxs-lookup"><span data-stu-id="952fa-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="952fa-189">El asistente de etiquetas Input, `Html.EditorFor` y `Html.TextBoxFor` están fuertemente tipados (usan expresiones lambda), pero `Html.TextBox` y `Html.Editor` no (usan nombres de expresión).</span><span class="sxs-lookup"><span data-stu-id="952fa-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="952fa-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="952fa-190">HtmlAttributes</span></span>

<span data-ttu-id="952fa-191">`@Html.Editor()` y `@Html.EditorFor()` usan una entrada `ViewDataDictionary` especial denominada `htmlAttributes` al ejecutar sus plantillas predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="952fa-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="952fa-192">Si lo desea, este comportamiento se puede enriquecer con parámetros `additionalViewData`.</span><span class="sxs-lookup"><span data-stu-id="952fa-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="952fa-193">En la clave "htmlAttributes" se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="952fa-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="952fa-194">La clave "htmlAttributes" se controla de forma similar al objeto `htmlAttributes` pasado a asistentes de etiquetas Input como `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="952fa-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="952fa-195">Nombres de expresión</span><span class="sxs-lookup"><span data-stu-id="952fa-195">Expression names</span></span>

<span data-ttu-id="952fa-196">El valor del atributo `asp-for` es una `ModelExpression` y la parte de la derecha de una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="952fa-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="952fa-197">Por tanto, `asp-for="Property1"` se convierte en `m => m.Property1` en el código generado, motivo por el que no es necesario incluir el prefijo `Model`.</span><span class="sxs-lookup"><span data-stu-id="952fa-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="952fa-198">Puede usar el carácter "\@" para iniciar una expresión insertada y moverla antes de `m.`:</span><span class="sxs-lookup"><span data-stu-id="952fa-198">You can use the "\@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="952fa-199">Se genera el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="952fa-200">Con las propiedades de colección, `asp-for="CollectionProperty[23].Member"` genera el mismo nombre que `asp-for="CollectionProperty[i].Member"` si `i` tiene el valor `23`.</span><span class="sxs-lookup"><span data-stu-id="952fa-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="952fa-201">Cuando ASP.NET Core MVC calcula el valor de `ModelExpression`, inspecciona varios orígenes, `ModelState` incluido.</span><span class="sxs-lookup"><span data-stu-id="952fa-201">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="952fa-202">Fíjese en `<input type="text" asp-for="@Name" />`.</span><span class="sxs-lookup"><span data-stu-id="952fa-202">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="952fa-203">El atributo `value` calculado es el primer valor distinto de null de:</span><span class="sxs-lookup"><span data-stu-id="952fa-203">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="952fa-204">La entrada `ModelState` con la clave "Name".</span><span class="sxs-lookup"><span data-stu-id="952fa-204">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="952fa-205">El resultado de la expresión `Model.Name`.</span><span class="sxs-lookup"><span data-stu-id="952fa-205">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="952fa-206">Navegar a las propiedades secundarias</span><span class="sxs-lookup"><span data-stu-id="952fa-206">Navigating child properties</span></span>

<span data-ttu-id="952fa-207">También se puede navegar a las propiedades secundarias a través de la ruta de acceso de propiedades del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="952fa-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="952fa-208">Pensemos en una clase de modelo más compleja que contiene una propiedad secundaria `Address`.</span><span class="sxs-lookup"><span data-stu-id="952fa-208">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="952fa-209">En la vista, enlazamos a `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="952fa-209">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="952fa-210">Se genera el siguiente código HTML para `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="952fa-210">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="952fa-211">Colecciones y nombres de expresión</span><span class="sxs-lookup"><span data-stu-id="952fa-211">Expression names and Collections</span></span>

<span data-ttu-id="952fa-212">Como ejemplo, un modelo que contiene una matriz de `Colors`:</span><span class="sxs-lookup"><span data-stu-id="952fa-212">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="952fa-213">El método de acción:</span><span class="sxs-lookup"><span data-stu-id="952fa-213">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="952fa-214">El siguiente código de Razor muestra cómo se tiene acceso a un elemento `Color` concreto:</span><span class="sxs-lookup"><span data-stu-id="952fa-214">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="952fa-215">La plantilla *Views/Shared/EditorTemplates/String.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="952fa-215">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="952fa-216">Ejemplo en el que se usa `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="952fa-216">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="952fa-217">El siguiente código de Razor muestra cómo iterar por una colección:</span><span class="sxs-lookup"><span data-stu-id="952fa-217">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="952fa-218">La plantilla *Views/Shared/EditorTemplates/ToDoItem.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="952fa-218">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

<span data-ttu-id="952fa-219">Si es posible, debe usarse `foreach` si el valor se va a utilizar en un contexto equivalente a `asp-for` o `Html.DisplayFor`.</span><span class="sxs-lookup"><span data-stu-id="952fa-219">`foreach` should be used if possible when the value is going to be used in an `asp-for` or `Html.DisplayFor` equivalent context.</span></span> <span data-ttu-id="952fa-220">En general, `for` es mejor que `foreach` (si el escenario lo permite), ya que no necesita asignar ningún enumerador; sin embargo, la evaluación de un indizador en una expresión LINQ puede resultar caro y, por tanto, se debe minimizar.</span><span class="sxs-lookup"><span data-stu-id="952fa-220">In general, `for` is better than `foreach` (if the scenario allows it) because it doesn't need to allocate an enumerator; however, evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="952fa-221">El código de ejemplo comentado anterior muestra cómo reemplazaríamos la expresión lambda por el operador `@` para tener acceso a cada elemento `ToDoItem` de la lista.</span><span class="sxs-lookup"><span data-stu-id="952fa-221">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="952fa-222">Asistente de etiquetas de área de texto (Textarea)</span><span class="sxs-lookup"><span data-stu-id="952fa-222">The Textarea Tag Helper</span></span>

<span data-ttu-id="952fa-223">El asistente de etiquetas `Textarea Tag Helper` es similar al asistente de etiquetas Input.</span><span class="sxs-lookup"><span data-stu-id="952fa-223">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="952fa-224">Genera los atributos `id` y `name`, y los atributos de validación de datos del modelo de un elemento [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea).</span><span class="sxs-lookup"><span data-stu-id="952fa-224">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="952fa-225">Permite establecer tipado fuerte.</span><span class="sxs-lookup"><span data-stu-id="952fa-225">Provides strong typing.</span></span>

* <span data-ttu-id="952fa-226">Alternativa del asistente de HTML: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="952fa-226">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="952fa-227">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-227">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="952fa-228">Se genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-228">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="952fa-229">Asistente de etiquetas Label</span><span class="sxs-lookup"><span data-stu-id="952fa-229">The Label Tag Helper</span></span>

* <span data-ttu-id="952fa-230">Genera el título de la etiqueta y el atributo `for` en un elemento [<label>](https://www.w3.org/wiki/HTML/Elements/label) de un nombre de expresión.</span><span class="sxs-lookup"><span data-stu-id="952fa-230">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="952fa-231">Alternativa del asistente de HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="952fa-231">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="952fa-232">`Label Tag Helper` proporciona las siguientes ventajas con respecto a un elemento HTML Label puro:</span><span class="sxs-lookup"><span data-stu-id="952fa-232">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="952fa-233">Obtendrá automáticamente el valor de la etiqueta descriptiva del atributo `Display`.</span><span class="sxs-lookup"><span data-stu-id="952fa-233">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="952fa-234">El nombre para mostrar que se busca puede cambiar con el tiempo y la combinación del atributo `Display`, y el asistente de etiquetas Label aplicará el elemento `Display` en cualquier lugar donde se use.</span><span class="sxs-lookup"><span data-stu-id="952fa-234">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="952fa-235">El código fuente contiene menos marcado.</span><span class="sxs-lookup"><span data-stu-id="952fa-235">Less markup in source code</span></span>

* <span data-ttu-id="952fa-236">Permite establecer tipado fuerte con la propiedad de modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-236">Strong typing with the model property.</span></span>

<span data-ttu-id="952fa-237">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-237">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="952fa-238">Se genera el siguiente código HTML para el elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="952fa-238">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="952fa-239">El asistente de etiquetas Label genera el valor de atributo `for` de "Email", que es el identificador asociado al elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="952fa-239">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="952fa-240">Los asistentes de etiquetas generan elementos `id` y `for` coherentes para que se puedan asociar correctamente.</span><span class="sxs-lookup"><span data-stu-id="952fa-240">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="952fa-241">El título de este ejemplo proviene del atributo `Display`.</span><span class="sxs-lookup"><span data-stu-id="952fa-241">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="952fa-242">Si el modelo no contuviera un atributo `Display`, el título sería el nombre de propiedad de la expresión.</span><span class="sxs-lookup"><span data-stu-id="952fa-242">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="952fa-243">Asistentes de etiquetas de validación</span><span class="sxs-lookup"><span data-stu-id="952fa-243">The Validation Tag Helpers</span></span>

<span data-ttu-id="952fa-244">Hay dos asistentes de etiquetas de validación.</span><span class="sxs-lookup"><span data-stu-id="952fa-244">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="952fa-245">`Validation Message Tag Helper` (que muestra un mensaje de validación relativo a una única propiedad del modelo) y `Validation Summary Tag Helper` (que muestra un resumen de los errores de validación).</span><span class="sxs-lookup"><span data-stu-id="952fa-245">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="952fa-246">`Input Tag Helper` agrega atributos de validación del lado cliente HTML5 a los elementos de entrada en función de los atributos de anotación de datos de las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-246">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="952fa-247">La validación también se realiza en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="952fa-247">Validation is also performed on the server.</span></span> <span data-ttu-id="952fa-248">El asistente de etiquetas de validación muestra estos mensajes de error cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="952fa-248">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="952fa-249">Asistente de etiquetas de mensaje de validación</span><span class="sxs-lookup"><span data-stu-id="952fa-249">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="952fa-250">Agrega el atributo [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` al elemento [span](https://developer.mozilla.org/docs/Web/HTML/Element/span), que adjunta los mensajes de error de validación en el campo de entrada de la propiedad de modelo especificada.</span><span class="sxs-lookup"><span data-stu-id="952fa-250">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="952fa-251">Cuando se produce un error de validación en el lado cliente, [jQuery](https://jquery.com/) muestra el mensaje de error en el elemento `<span>`.</span><span class="sxs-lookup"><span data-stu-id="952fa-251">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="952fa-252">La validación también tiene lugar en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="952fa-252">Validation also takes place on the server.</span></span> <span data-ttu-id="952fa-253">Puede que JavaScript esté deshabilitado en los clientes, mientras que hay algunas validaciones que solo se pueden realizar en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="952fa-253">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="952fa-254">Alternativa del asistente de HTML: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="952fa-254">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="952fa-255">`Validation Message Tag Helper` se usa con el atributo `asp-validation-for` en un elemento HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span).</span><span class="sxs-lookup"><span data-stu-id="952fa-255">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="952fa-256">El asistente de etiquetas de mensaje de validación generará el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-256">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="952fa-257">`Validation Message Tag Helper` suele usarse por lo general después de un asistente de etiquetas `Input` en la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="952fa-257">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="952fa-258">Gracias a esto, se mostrarán todos los mensajes de error de validación cerca de la entrada que produjo el error.</span><span class="sxs-lookup"><span data-stu-id="952fa-258">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="952fa-259">Debe tener una vista con las referencias de script de JavaScript y de [jQuery](https://jquery.com/) adecuadas para la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="952fa-259">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="952fa-260">Para más información, vea [Introduction to model validation in ASP.NET Core MVC](../models/validation.md) (Introducción a la validación de modelos en ASP.NET Core MVC).</span><span class="sxs-lookup"><span data-stu-id="952fa-260">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="952fa-261">Cuando se produce un error de validación del lado servidor (por ejemplo, porque haya una validación del lado servidor personalizada o porque la validación del lado cliente esté deshabilitada), MVC pone ese mensaje de error como cuerpo del elemento `<span>`.</span><span class="sxs-lookup"><span data-stu-id="952fa-261">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="952fa-262">Asistente de etiquetas de resumen de validación</span><span class="sxs-lookup"><span data-stu-id="952fa-262">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="952fa-263">Tiene como destino todos los elementos `<div>` con el atributo `asp-validation-summary`.</span><span class="sxs-lookup"><span data-stu-id="952fa-263">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="952fa-264">Alternativa del asistente de HTML: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="952fa-264">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="952fa-265">`Validation Summary Tag Helper` se usa para mostrar un resumen de los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="952fa-265">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="952fa-266">El valor de atributo `asp-validation-summary` puede ser cualquiera de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="952fa-266">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="952fa-267">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="952fa-267">asp-validation-summary</span></span>|<span data-ttu-id="952fa-268">Mensajes de validación que se muestran</span><span class="sxs-lookup"><span data-stu-id="952fa-268">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="952fa-269">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="952fa-269">ValidationSummary.All</span></span>|<span data-ttu-id="952fa-270">Nivel de modelo y de propiedad</span><span class="sxs-lookup"><span data-stu-id="952fa-270">Property and model level</span></span>|
|<span data-ttu-id="952fa-271">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="952fa-271">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="952fa-272">Modelo</span><span class="sxs-lookup"><span data-stu-id="952fa-272">Model</span></span>|
|<span data-ttu-id="952fa-273">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="952fa-273">ValidationSummary.None</span></span>|<span data-ttu-id="952fa-274">Ninguna</span><span class="sxs-lookup"><span data-stu-id="952fa-274">None</span></span>|

### <a name="sample"></a><span data-ttu-id="952fa-275">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="952fa-275">Sample</span></span>

<span data-ttu-id="952fa-276">En el siguiente ejemplo, el modelo de datos se complementa con atributos `DataAnnotation`, lo que genera mensajes de error de validación sobre el elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="952fa-276">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="952fa-277">Cuando se produce un error de validación, el asistente de etiquetas de validación muestra el mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="952fa-277">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="952fa-278">El código HTML generado (cuando el modelo es válido) es este:</span><span class="sxs-lookup"><span data-stu-id="952fa-278">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="952fa-279">Asistente de etiquetas de selección (Select)</span><span class="sxs-lookup"><span data-stu-id="952fa-279">The Select Tag Helper</span></span>

* <span data-ttu-id="952fa-280">Genera el elemento [select](https://www.w3.org/wiki/HTML/Elements/select) y el elemento asociado [option](https://www.w3.org/wiki/HTML/Elements/option) de las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="952fa-280">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="952fa-281">Tiene `Html.DropDownListFor` y `Html.ListBoxFor` como alternativa del asistente de HTML.</span><span class="sxs-lookup"><span data-stu-id="952fa-281">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="952fa-282">El elemento `asp-for` de `Select Tag Helper` especifica el nombre de la propiedad de modelo del elemento [select](https://www.w3.org/wiki/HTML/Elements/select), mientras que `asp-items` especifica los elementos [option](https://www.w3.org/wiki/HTML/Elements/option).</span><span class="sxs-lookup"><span data-stu-id="952fa-282">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="952fa-283">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-283">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="952fa-284">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-284">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="952fa-285">El método `Index` inicializa `CountryViewModel`, establece el país seleccionado y lo pasa a la vista `Index`.</span><span class="sxs-lookup"><span data-stu-id="952fa-285">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

<span data-ttu-id="952fa-286">El método HTTP POST `Index` muestra la selección:</span><span class="sxs-lookup"><span data-stu-id="952fa-286">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="952fa-287">La vista `Index`:</span><span class="sxs-lookup"><span data-stu-id="952fa-287">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="952fa-288">Genera el siguiente código HTML (con la selección "CA"):</span><span class="sxs-lookup"><span data-stu-id="952fa-288">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="952fa-289">No se recomienda usar `ViewBag` o `ViewData` con el asistente de etiquetas Select.</span><span class="sxs-lookup"><span data-stu-id="952fa-289">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="952fa-290">Un modelo de vista es más eficaz a la hora de proporcionar metadatos MVC y suele ser menos problemático.</span><span class="sxs-lookup"><span data-stu-id="952fa-290">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="952fa-291">El valor de atributo `asp-for` es un caso especial y no necesita un prefijo `Model`, mientras que los otros atributos del asistente de etiquetas sí (como `asp-items`).</span><span class="sxs-lookup"><span data-stu-id="952fa-291">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="952fa-292">Enlace con enum</span><span class="sxs-lookup"><span data-stu-id="952fa-292">Enum binding</span></span>

<span data-ttu-id="952fa-293">A menudo, conviene usar `<select>` con una propiedad `enum` y generar los elementos `SelectListItem` a partir de valores `enum`.</span><span class="sxs-lookup"><span data-stu-id="952fa-293">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="952fa-294">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="952fa-294">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="952fa-295">El método `GetEnumSelectList` genera un objeto `SelectList` para una enumeración.</span><span class="sxs-lookup"><span data-stu-id="952fa-295">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="952fa-296">Puede complementar la lista de enumeradores con el atributo `Display` para obtener una interfaz de usuario más completa:</span><span class="sxs-lookup"><span data-stu-id="952fa-296">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="952fa-297">Se genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-297">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="952fa-298">Agrupamiento de opciones</span><span class="sxs-lookup"><span data-stu-id="952fa-298">Option Group</span></span>

<span data-ttu-id="952fa-299">El elemento HTML [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) se genera cuando el modelo de vista contiene uno o varios objetos `SelectListGroup`.</span><span class="sxs-lookup"><span data-stu-id="952fa-299">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="952fa-300">`CountryViewModelGroup` agrupa los elementos `SelectListItem` en los grupos "North America" y "Europe":</span><span class="sxs-lookup"><span data-stu-id="952fa-300">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="952fa-301">Aquí mostramos los dos grupos:</span><span class="sxs-lookup"><span data-stu-id="952fa-301">The two groups are shown below:</span></span>

![Ejemplo de agrupamiento de opciones](working-with-forms/_static/grp.png)

<span data-ttu-id="952fa-303">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="952fa-303">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="952fa-304">Selección múltiple</span><span class="sxs-lookup"><span data-stu-id="952fa-304">Multiple select</span></span>

<span data-ttu-id="952fa-305">El asistente de etiquetas Select generará automáticamente el atributo [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) si la propiedad especificada en el atributo `asp-for` es `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="952fa-305">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="952fa-306">Por ejemplo, si tenemos el siguiente modelo:</span><span class="sxs-lookup"><span data-stu-id="952fa-306">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="952fa-307">Con la siguiente vista:</span><span class="sxs-lookup"><span data-stu-id="952fa-307">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="952fa-308">Se genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-308">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="952fa-309">Sin selección</span><span class="sxs-lookup"><span data-stu-id="952fa-309">No selection</span></span>

<span data-ttu-id="952fa-310">Si ve que usa la opción "sin especificar" en varias páginas, puede crear una plantilla para no tener que repetir el código HTML:</span><span class="sxs-lookup"><span data-stu-id="952fa-310">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="952fa-311">La plantilla *Views/Shared/EditorTemplates/CountryViewModel.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="952fa-311">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="952fa-312">La posibilidad de agregar elementos HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) no se limita exclusivamente a los casos en los que no se *seleccionada nada*.</span><span class="sxs-lookup"><span data-stu-id="952fa-312">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="952fa-313">Por ejemplo, el método de acción y vista siguientes generarán un código HTML similar al código anterior:</span><span class="sxs-lookup"><span data-stu-id="952fa-313">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="952fa-314">Se seleccionará el elemento `<option>` correcto (que contenga el atributo `selected="selected"`) en función del valor real de `Country`.</span><span class="sxs-lookup"><span data-stu-id="952fa-314">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="952fa-315">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="952fa-315">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <span data-ttu-id="952fa-316">[HTML Form element](https://www.w3.org/TR/html401/interact/forms.html) (Elemento HTML Form)</span><span class="sxs-lookup"><span data-stu-id="952fa-316">[HTML Form element](https://www.w3.org/TR/html401/interact/forms.html)</span></span>
* <span data-ttu-id="952fa-317">[Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) (Token de comprobación de solicitudes)</span><span class="sxs-lookup"><span data-stu-id="952fa-317">[Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)</span></span>
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* <span data-ttu-id="952fa-318">[IAttributeAdapter Interface](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter) (Interfaz IAttributeAdapter)</span><span class="sxs-lookup"><span data-stu-id="952fa-318">[IAttributeAdapter Interface](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)</span></span>
* [<span data-ttu-id="952fa-319">Fragmentos de código de este documento</span><span class="sxs-lookup"><span data-stu-id="952fa-319">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
