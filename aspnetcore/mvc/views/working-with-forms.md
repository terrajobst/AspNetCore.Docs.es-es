---
title: Aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core
author: rick-anderson
description: Describe la integrada de aplicaciones auxiliares de etiquetas que se utilizan con formularios.
keywords: "Núcleo de ASP.NET, aplicación auxiliar de etiquetas, TagHelper, formulario de HTML, formularios"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff6fee6eee539fc77b6c6180a816daa760202848
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="4139b-104">Introducción al uso de aplicaciones auxiliares de etiquetas en formularios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4139b-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="4139b-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), y [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="4139b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="4139b-106">Este documento muestra cómo trabajar con formularios y los elementos HTML que se utilizan habitualmente en un formulario.</span><span class="sxs-lookup"><span data-stu-id="4139b-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="4139b-107">El código HTML [formulario](https://www.w3.org/TR/html401/interact/forms.html) elemento proporciona el uso de aplicaciones web mecanismo principal para enviar datos al servidor.</span><span class="sxs-lookup"><span data-stu-id="4139b-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="4139b-108">La mayor parte de este documento describe [aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) y cómo puede ayudarle a crear productiva sólidos formularios HTML.</span><span class="sxs-lookup"><span data-stu-id="4139b-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="4139b-109">Le recomendamos que lea [Introducción a las aplicaciones auxiliares de etiquetas](tag-helpers/intro.md) antes de leer este documento.</span><span class="sxs-lookup"><span data-stu-id="4139b-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="4139b-110">En muchos casos, las aplicaciones auxiliares HTML proporcionan un enfoque alternativo para una aplicación auxiliar de etiqueta específico, pero es importante reconocer que aplicaciones auxiliares de etiquetas no reemplazan métodos auxiliares HTML y no es una aplicación auxiliar de etiquetas para cada aplicación auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="4139b-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="4139b-111">Cuando existe una alternativa de la aplicación auxiliar HTML, se menciona.</span><span class="sxs-lookup"><span data-stu-id="4139b-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="4139b-112">La aplicación auxiliar de etiqueta de formulario</span><span class="sxs-lookup"><span data-stu-id="4139b-112">The Form Tag Helper</span></span>

<span data-ttu-id="4139b-113">El [formulario](https://www.w3.org/TR/html401/interact/forms.html) auxiliar de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="4139b-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="4139b-114">Genera el código HTML [ \<formulario >](https://www.w3.org/TR/html401/interact/forms.html) `action` valor de atributo para una acción de controlador MVC o la ruta con nombre</span><span class="sxs-lookup"><span data-stu-id="4139b-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="4139b-115">Genera oculto [comprobación de solicitud de Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para impedir la falsificación de solicitud entre sitios (cuando se usa con el `[ValidateAntiForgeryToken]` atributo en el método de acción HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="4139b-115">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="4139b-116">Proporciona el `asp-route-<Parameter Name>` atributo, donde `<Parameter Name>` se agrega a los valores de ruta.</span><span class="sxs-lookup"><span data-stu-id="4139b-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="4139b-117">El `routeValues` parámetros `Html.BeginForm` y `Html.BeginRouteForm` proporcionan una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="4139b-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="4139b-118">Una alternativa de la aplicación auxiliar HTML `Html.BeginForm` y`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="4139b-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="4139b-119">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-119">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="4139b-120">El Ayudante de etiqueta de formulario anterior genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-120">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="4139b-121">El tiempo de ejecución MVC genera el `action` valor del atributo de los atributos de la aplicación auxiliar de etiqueta de formulario `asp-controller` y `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="4139b-121">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="4139b-122">La aplicación auxiliar de etiqueta de formulario también genera oculto [comprobación de solicitud de Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para impedir la falsificación de solicitud entre sitios (cuando se usa con el `[ValidateAntiForgeryToken]` atributo en el método de acción HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="4139b-122">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="4139b-123">Protección de un formulario HTML puro de falsificación de solicitudes entre sitios es difícil, la aplicación auxiliar de etiqueta de formulario proporciona este servicio.</span><span class="sxs-lookup"><span data-stu-id="4139b-123">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="4139b-124">Uso de una ruta con nombre</span><span class="sxs-lookup"><span data-stu-id="4139b-124">Using a named route</span></span>

<span data-ttu-id="4139b-125">El `asp-route` atributo de aplicación auxiliar de etiquetas también puede generar el marcado para el código HTML `action` atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-125">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="4139b-126">Una aplicación con un [ruta](../../fundamentals/routing.md) denominado `register` podría usar el siguiente marcado de la página de registro:</span><span class="sxs-lookup"><span data-stu-id="4139b-126">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="4139b-127">Muchas de las vistas en el *Views/Account* carpeta (generan cuando se crea una nueva aplicación web con *cuentas de usuario individuales*) contienen el [returnurl de ruta de asp](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atributo:</span><span class="sxs-lookup"><span data-stu-id="4139b-127">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="4139b-128">Con las plantillas incorporadas, `returnUrl` solo se rellena automáticamente cuando intenta obtener acceso a un recurso autorizado, pero no son autentica o autoriza.</span><span class="sxs-lookup"><span data-stu-id="4139b-128">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="4139b-129">Si trata de un acceso no autorizado, el middleware de seguridad le redirige a la página de inicio de sesión con el `returnUrl` establecido.</span><span class="sxs-lookup"><span data-stu-id="4139b-129">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="4139b-130">La aplicación auxiliar de la etiqueta de entrada</span><span class="sxs-lookup"><span data-stu-id="4139b-130">The Input Tag Helper</span></span>

<span data-ttu-id="4139b-131">La aplicación auxiliar de etiqueta de entrada se enlaza a un elemento HTML [ \<entrada >](https://www.w3.org/wiki/HTML/Elements/input) elemento a una expresión de modelo en la vista de razor.</span><span class="sxs-lookup"><span data-stu-id="4139b-131">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="4139b-132">Sintaxis:</span><span class="sxs-lookup"><span data-stu-id="4139b-132">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="4139b-133">La aplicación auxiliar de etiqueta de entrada:</span><span class="sxs-lookup"><span data-stu-id="4139b-133">The Input Tag Helper:</span></span>

* <span data-ttu-id="4139b-134">Genera el `id` y `name` atributos HTML para el nombre de la expresión especificada en el `asp-for` atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-134">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="4139b-135">`asp-for="Property1.Property2"` es equivalente a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="4139b-135">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="4139b-136">El nombre de la expresión es lo que se usa para la `asp-for` valor de atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-136">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="4139b-137">Consulte la [nombres de la expresión](#expression-names) sección para obtener información adicional.</span><span class="sxs-lookup"><span data-stu-id="4139b-137">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="4139b-138">Establece el código HTML `type` según el tipo de modelo de valor de atributo y [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados a la propiedad de modelo</span><span class="sxs-lookup"><span data-stu-id="4139b-138">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="4139b-139">No se sobrescribirá el código HTML `type` valor de atributo cuando se especifica uno</span><span class="sxs-lookup"><span data-stu-id="4139b-139">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="4139b-140">Genera [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributos de validación de [anotación de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados a las propiedades del modelo</span><span class="sxs-lookup"><span data-stu-id="4139b-140">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="4139b-141">Tiene una característica de la aplicación auxiliar HTML que se superponen con `Html.TextBoxFor` y `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="4139b-141">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="4139b-142">Consulte la **alternativas de la aplicación auxiliar HTML para la aplicación auxiliar de etiqueta de entrada** sección para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4139b-142">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="4139b-143">Proporciona el establecimiento inflexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="4139b-143">Provides strong typing.</span></span> <span data-ttu-id="4139b-144">Si el nombre de los cambios de propiedad y no actualiza la aplicación auxiliar de etiqueta obtendrá un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="4139b-144">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="4139b-145">El `Input` etiqueta auxiliar establece el código HTML `type` atributo basado en el tipo. NET.</span><span class="sxs-lookup"><span data-stu-id="4139b-145">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="4139b-146">En la tabla siguiente se enumera algunos tipos comunes de .NET y el tipo HTML generado (no todos los tipos .NET aparece).</span><span class="sxs-lookup"><span data-stu-id="4139b-146">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="4139b-147">Tipo de .NET</span><span class="sxs-lookup"><span data-stu-id="4139b-147">.NET type</span></span>|<span data-ttu-id="4139b-148">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="4139b-148">Input Type</span></span>|
|---|---|
|<span data-ttu-id="4139b-149">Bool</span><span class="sxs-lookup"><span data-stu-id="4139b-149">Bool</span></span>|<span data-ttu-id="4139b-150">tipo = "checkbox"</span><span class="sxs-lookup"><span data-stu-id="4139b-150">type=”checkbox”</span></span>|
|<span data-ttu-id="4139b-151">Cadena</span><span class="sxs-lookup"><span data-stu-id="4139b-151">String</span></span>|<span data-ttu-id="4139b-152">tipo = "text"</span><span class="sxs-lookup"><span data-stu-id="4139b-152">type=”text”</span></span>|
|<span data-ttu-id="4139b-153">DateTime</span><span class="sxs-lookup"><span data-stu-id="4139b-153">DateTime</span></span>|<span data-ttu-id="4139b-154">tipo = "datetime"</span><span class="sxs-lookup"><span data-stu-id="4139b-154">type=”datetime”</span></span>|
|<span data-ttu-id="4139b-155">Byte</span><span class="sxs-lookup"><span data-stu-id="4139b-155">Byte</span></span>|<span data-ttu-id="4139b-156">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="4139b-156">type=”number”</span></span>|
|<span data-ttu-id="4139b-157">Valor int.</span><span class="sxs-lookup"><span data-stu-id="4139b-157">Int</span></span>|<span data-ttu-id="4139b-158">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="4139b-158">type=”number”</span></span>|
|<span data-ttu-id="4139b-159">Single, Double</span><span class="sxs-lookup"><span data-stu-id="4139b-159">Single, Double</span></span>|<span data-ttu-id="4139b-160">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="4139b-160">type=”number”</span></span>|


<span data-ttu-id="4139b-161">La siguiente tabla muestra algunos común [las anotaciones de datos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos que la aplicación auxiliar de la etiqueta de entrada se asigne a tipos de entrada específicos (no cada atributo de validación se muestra):</span><span class="sxs-lookup"><span data-stu-id="4139b-161">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="4139b-162">Atributo</span><span class="sxs-lookup"><span data-stu-id="4139b-162">Attribute</span></span>|<span data-ttu-id="4139b-163">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="4139b-163">Input Type</span></span>|
|---|---|
|<span data-ttu-id="4139b-164">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="4139b-164">[EmailAddress]</span></span>|<span data-ttu-id="4139b-165">tipo = "email"</span><span class="sxs-lookup"><span data-stu-id="4139b-165">type=”email”</span></span>|
|<span data-ttu-id="4139b-166">[Url]</span><span class="sxs-lookup"><span data-stu-id="4139b-166">[Url]</span></span>|<span data-ttu-id="4139b-167">tipo = "url"</span><span class="sxs-lookup"><span data-stu-id="4139b-167">type=”url”</span></span>|
|<span data-ttu-id="4139b-168">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="4139b-168">[HiddenInput]</span></span>|<span data-ttu-id="4139b-169">tipo = "hidden"</span><span class="sxs-lookup"><span data-stu-id="4139b-169">type=”hidden”</span></span>|
|<span data-ttu-id="4139b-170">[Phone]</span><span class="sxs-lookup"><span data-stu-id="4139b-170">[Phone]</span></span>|<span data-ttu-id="4139b-171">tipo = "tel"</span><span class="sxs-lookup"><span data-stu-id="4139b-171">type=”tel”</span></span>|
|<span data-ttu-id="4139b-172">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="4139b-172">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="4139b-173">tipo = "contraseña"</span><span class="sxs-lookup"><span data-stu-id="4139b-173">type=”password”</span></span>|
|<span data-ttu-id="4139b-174">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="4139b-174">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="4139b-175">tipo = "fecha"</span><span class="sxs-lookup"><span data-stu-id="4139b-175">type=”date”</span></span>|
|<span data-ttu-id="4139b-176">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="4139b-176">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="4139b-177">tipo = "hora"</span><span class="sxs-lookup"><span data-stu-id="4139b-177">type=”time”</span></span>|


<span data-ttu-id="4139b-178">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-178">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="4139b-179">El código anterior genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-179">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
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

<span data-ttu-id="4139b-180">Las anotaciones de datos que se aplica a la `Email` y `Password` propiedades generan metadatos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="4139b-180">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="4139b-181">La aplicación auxiliar de etiqueta de entrada usa los metadatos del modelo y produce [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributos (vea [validación del modelo](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="4139b-181">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="4139b-182">Estos atributos describen los validadores para adjuntar a los campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="4139b-182">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="4139b-183">Esto proporciona HTML5 discreto y [jQuery](https://jquery.com/) validación.</span><span class="sxs-lookup"><span data-stu-id="4139b-183">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="4139b-184">Los atributos discretos tienen el formato `data-val-rule="Error Message"`, donde la regla es el nombre de la regla de validación (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etcetera.) Si un mensaje de error se proporciona en el atributo, se muestra como el valor de la `data-val-rule` atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-184">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="4139b-185">También hay atributos del formulario `data-val-ruleName-argumentName="argumentValue"` que proporcionan detalles adicionales sobre la regla, por ejemplo, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="4139b-185">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="4139b-186">Alternativas de la aplicación auxiliar HTML para la aplicación auxiliar de etiqueta de entrada</span><span class="sxs-lookup"><span data-stu-id="4139b-186">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="4139b-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` y `Html.EditorFor` tienen características a la aplicación auxiliar de etiqueta de entrada que se superponen.</span><span class="sxs-lookup"><span data-stu-id="4139b-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="4139b-188">La aplicación auxiliar de etiqueta de entrada se establecerá automáticamente el `type` atributo; `Html.TextBox` y `Html.TextBoxFor` no.</span><span class="sxs-lookup"><span data-stu-id="4139b-188">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="4139b-189">`Html.Editor`y `Html.EditorFor` controlar colecciones, objetos complejos y plantillas; no lo hace la aplicación auxiliar de etiqueta de entrada.</span><span class="sxs-lookup"><span data-stu-id="4139b-189">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="4139b-190">La aplicación auxiliar etiqueta de entrada, `Html.EditorFor` y `Html.TextBoxFor` están fuertemente tipados (usar expresiones lambda;) `Html.TextBox` y `Html.Editor` no son (usan nombres de la expresión).</span><span class="sxs-lookup"><span data-stu-id="4139b-190">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="4139b-191">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="4139b-191">HtmlAttributes</span></span>

<span data-ttu-id="4139b-192">`@Html.Editor()`y `@Html.EditorFor()` usar una clase especial `ViewDataDictionary` entrada denominada `htmlAttributes` al ejecutar sus plantillas predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="4139b-192">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="4139b-193">Este comportamiento es aumentar opcionalmente utilizando `additionalViewData` parámetros.</span><span class="sxs-lookup"><span data-stu-id="4139b-193">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="4139b-194">La clave "htmlAttributes" distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4139b-194">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="4139b-195">La clave "htmlAttributes" se controla de forma similar a la `htmlAttributes` objeto pasado como entrada aplicaciones auxiliares como `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="4139b-195">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="4139b-196">Nombres de la expresión</span><span class="sxs-lookup"><span data-stu-id="4139b-196">Expression names</span></span>

<span data-ttu-id="4139b-197">El `asp-for` valor del atributo es un `ModelExpression` y el lado derecho de una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="4139b-197">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="4139b-198">Por lo tanto, `asp-for="Property1"` se convierte en `m => m.Property1` en el código generado, motivo por el que no es necesario con el prefijo `Model`.</span><span class="sxs-lookup"><span data-stu-id="4139b-198">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="4139b-199">Puede usar el "@" caracteres para iniciar una expresión en línea y mover antes el `m.`:</span><span class="sxs-lookup"><span data-stu-id="4139b-199">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="4139b-200">Genera lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4139b-200">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="4139b-201">Con las propiedades de colección, `asp-for="CollectionProperty[23].Member"` genera el mismo nombre que `asp-for="CollectionProperty[i].Member"` cuando `i` tiene el valor `23`.</span><span class="sxs-lookup"><span data-stu-id="4139b-201">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="4139b-202">Navegar por propiedades secundarias</span><span class="sxs-lookup"><span data-stu-id="4139b-202">Navigating child properties</span></span>

<span data-ttu-id="4139b-203">También puede navegar a propiedades secundarias mediante la ruta de acceso de propiedad del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="4139b-203">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="4139b-204">Considere la posibilidad de una clase de modelo más compleja que contiene un elemento secundario `Address` propiedad.</span><span class="sxs-lookup"><span data-stu-id="4139b-204">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="4139b-205">En la vista, se enlazan a `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="4139b-205">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="4139b-206">El siguiente código HTML se genera para `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="4139b-206">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="4139b-207">Nombres de la expresión y colecciones</span><span class="sxs-lookup"><span data-stu-id="4139b-207">Expression names and Collections</span></span>

<span data-ttu-id="4139b-208">Ejemplo de un modelo que contiene una matriz de `Colors`:</span><span class="sxs-lookup"><span data-stu-id="4139b-208">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="4139b-209">El método de acción:</span><span class="sxs-lookup"><span data-stu-id="4139b-209">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="4139b-210">El código Razor siguiente muestra cómo tener acceso a un determinado `Color` elemento:</span><span class="sxs-lookup"><span data-stu-id="4139b-210">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="4139b-211">El *Views/Shared/EditorTemplates/String.cshtml* plantilla:</span><span class="sxs-lookup"><span data-stu-id="4139b-211">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="4139b-212">Uso de ejemplo `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="4139b-212">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="4139b-213">El código Razor siguiente muestra cómo recorrer en iteración una colección:</span><span class="sxs-lookup"><span data-stu-id="4139b-213">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="4139b-214">El *Views/Shared/EditorTemplates/ToDoItem.cshtml* plantilla:</span><span class="sxs-lookup"><span data-stu-id="4139b-214">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="4139b-215">Utilice siempre `for` (y *no* `foreach`) para recorrer en iteración una lista.</span><span class="sxs-lookup"><span data-stu-id="4139b-215">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="4139b-216">Evaluar un indizador en una LINQ expresión puede ser costosa y debe reducirse.</span><span class="sxs-lookup"><span data-stu-id="4139b-216">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="4139b-217">El código de ejemplo comentado anterior muestra cómo podría reemplazar la expresión lambda con la `@` operador para tener acceso a cada uno de ellos `ToDoItem` en la lista.</span><span class="sxs-lookup"><span data-stu-id="4139b-217">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="4139b-218">La aplicación auxiliar de etiqueta Textarea</span><span class="sxs-lookup"><span data-stu-id="4139b-218">The Textarea Tag Helper</span></span>

<span data-ttu-id="4139b-219">El `Textarea Tag Helper` auxiliar de etiqueta es similar a la aplicación auxiliar de etiqueta de entrada.</span><span class="sxs-lookup"><span data-stu-id="4139b-219">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="4139b-220">Genera el `id` y `name` atributos y los atributos de validación de datos del modelo para un [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-220">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="4139b-221">Proporciona el establecimiento inflexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="4139b-221">Provides strong typing.</span></span>

* <span data-ttu-id="4139b-222">Alternativa de la aplicación auxiliar HTML:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="4139b-222">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="4139b-223">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-223">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="4139b-224">Se genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-224">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="4139b-225">La aplicación auxiliar de etiqueta de etiqueta</span><span class="sxs-lookup"><span data-stu-id="4139b-225">The Label Tag Helper</span></span>

* <span data-ttu-id="4139b-226">Genera el título de la etiqueta y `for` del atributo en un [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) (elemento) para un nombre de expresión</span><span class="sxs-lookup"><span data-stu-id="4139b-226">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="4139b-227">Alternativa de la aplicación auxiliar HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="4139b-227">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="4139b-228">El `Label Tag Helper` proporciona las siguientes ventajas con respecto a un elemento label HTML puro:</span><span class="sxs-lookup"><span data-stu-id="4139b-228">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="4139b-229">Obtendrá automáticamente el valor de etiqueta descriptiva de la `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-229">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="4139b-230">Puede cambiar el nombre para mostrar deseado con el tiempo y la combinación de `Display` atributo y etiqueta etiqueta auxiliar se aplicarán la `Display` en cualquier lugar en el se utiliza.</span><span class="sxs-lookup"><span data-stu-id="4139b-230">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="4139b-231">Menos marcado en el código fuente</span><span class="sxs-lookup"><span data-stu-id="4139b-231">Less markup in source code</span></span>

* <span data-ttu-id="4139b-232">Establecimiento inflexible tipos con la propiedad de modelo.</span><span class="sxs-lookup"><span data-stu-id="4139b-232">Strong typing with the model property.</span></span>

<span data-ttu-id="4139b-233">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-233">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="4139b-234">El siguiente código HTML se genera para el `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="4139b-234">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="4139b-235">La aplicación auxiliar de la etiqueta etiqueta genera el `for` valor de atributo de "Email", que es el identificador asociado a la `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-235">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="4139b-236">Las aplicaciones auxiliares de etiqueta generar coherente `id` y `for` elementos, para que puedan asociados correctamente.</span><span class="sxs-lookup"><span data-stu-id="4139b-236">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="4139b-237">La descripción de este ejemplo proviene de la `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="4139b-237">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="4139b-238">Si el modelo no contiene un `Display` atributo, el título sería el nombre de propiedad de la expresión.</span><span class="sxs-lookup"><span data-stu-id="4139b-238">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="4139b-239">Aplicaciones auxiliares de etiquetas de validación</span><span class="sxs-lookup"><span data-stu-id="4139b-239">The Validation Tag Helpers</span></span>

<span data-ttu-id="4139b-240">Hay dos aplicaciones auxiliares de etiquetas de validación.</span><span class="sxs-lookup"><span data-stu-id="4139b-240">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="4139b-241">El `Validation Message Tag Helper` (que muestra un mensaje de validación de una sola propiedad en el modelo) y el `Validation Summary Tag Helper` (que muestra un resumen de errores de validación).</span><span class="sxs-lookup"><span data-stu-id="4139b-241">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="4139b-242">El `Input Tag Helper` agrega atributos de validación de lado de cliente de HTML5 para los elementos que basan en datos de los atributos de anotación en las clases de modelo de entrada.</span><span class="sxs-lookup"><span data-stu-id="4139b-242">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="4139b-243">También se realiza la validación en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4139b-243">Validation is also performed on the server.</span></span> <span data-ttu-id="4139b-244">La aplicación auxiliar de etiqueta de validación muestra estos mensajes de error cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="4139b-244">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="4139b-245">El Ayudante de etiqueta de mensaje de validación</span><span class="sxs-lookup"><span data-stu-id="4139b-245">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="4139b-246">Agrega el [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atribuir a la [abarcan](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, que asocia los mensajes de error de validación en el campo de entrada de la propiedad de modelo especificado.</span><span class="sxs-lookup"><span data-stu-id="4139b-246">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="4139b-247">Cuando se produce un error de validación del lado cliente, [jQuery](https://jquery.com/) muestra el mensaje de error en la `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-247">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="4139b-248">La validación también tiene lugar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4139b-248">Validation also takes place on the server.</span></span> <span data-ttu-id="4139b-249">Los clientes pueden tener JavaScript deshabilitado y alguna validación solo puede realizarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="4139b-249">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="4139b-250">Alternativa de la aplicación auxiliar HTML:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="4139b-250">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="4139b-251">El `Validation Message Tag Helper` se utiliza con la `asp-validation-for` atributo en una HTML [abarcan](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-251">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="4139b-252">El Ayudante de etiqueta de mensaje de validación generará el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-252">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="4139b-253">Se usa generalmente la `Validation Message Tag Helper` después de un `Input` auxiliar de etiqueta para la misma propiedad.</span><span class="sxs-lookup"><span data-stu-id="4139b-253">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="4139b-254">Si lo hace, muestra los mensajes de error de validación cerca de la entrada que produjo el error.</span><span class="sxs-lookup"><span data-stu-id="4139b-254">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="4139b-255">Debe tener una vista con el código de JavaScript correcto y [jQuery](https://jquery.com/) referencias en su lugar para la validación del lado cliente de script.</span><span class="sxs-lookup"><span data-stu-id="4139b-255">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="4139b-256">Vea [validación del modelo](../models/validation.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4139b-256">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="4139b-257">Cuando produce un error de validación del lado servidor (por ejemplo cuando tiene validación del lado servidor personalizado o de validación del lado cliente está deshabilitado), MVC coloca ese mensaje de error como el cuerpo de la `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-257">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="4139b-258">El Ayudante de validación del Summary (etiqueta)</span><span class="sxs-lookup"><span data-stu-id="4139b-258">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="4139b-259">Destinos `<div>` elementos con el `asp-validation-summary` atributo</span><span class="sxs-lookup"><span data-stu-id="4139b-259">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="4139b-260">Alternativa de la aplicación auxiliar HTML:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="4139b-260">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="4139b-261">El `Validation Summary Tag Helper` se usa para mostrar un resumen de mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="4139b-261">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="4139b-262">La `asp-validation-summary` valor de atributo puede ser cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="4139b-262">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="4139b-263">Resumen de validación de ASP</span><span class="sxs-lookup"><span data-stu-id="4139b-263">asp-validation-summary</span></span>|<span data-ttu-id="4139b-264">Mensajes de validación que se muestran</span><span class="sxs-lookup"><span data-stu-id="4139b-264">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="4139b-265">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="4139b-265">ValidationSummary.All</span></span>|<span data-ttu-id="4139b-266">Nivel de propiedad y el modelo</span><span class="sxs-lookup"><span data-stu-id="4139b-266">Property and model level</span></span>|
|<span data-ttu-id="4139b-267">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="4139b-267">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="4139b-268">Modelo</span><span class="sxs-lookup"><span data-stu-id="4139b-268">Model</span></span>|
|<span data-ttu-id="4139b-269">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="4139b-269">ValidationSummary.None</span></span>|<span data-ttu-id="4139b-270">Ninguna</span><span class="sxs-lookup"><span data-stu-id="4139b-270">None</span></span>|

### <a name="sample"></a><span data-ttu-id="4139b-271">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="4139b-271">Sample</span></span>

<span data-ttu-id="4139b-272">En el ejemplo siguiente, el modelo de datos se decora con `DataAnnotation` atributos, que genera mensajes de error de validación en el `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4139b-272">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="4139b-273">Cuando se produce un error de validación, la aplicación auxiliar de etiqueta de validación muestra el mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="4139b-273">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="4139b-274">El código HTML generado (cuando el modelo es válido):</span><span class="sxs-lookup"><span data-stu-id="4139b-274">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
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

## <a name="the-select-tag-helper"></a><span data-ttu-id="4139b-275">La aplicación auxiliar de la etiqueta Select</span><span class="sxs-lookup"><span data-stu-id="4139b-275">The Select Tag Helper</span></span>

* <span data-ttu-id="4139b-276">Genera [seleccione](https://www.w3.org/wiki/HTML/Elements/select) y [opción](https://www.w3.org/wiki/HTML/Elements/option) elementos para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="4139b-276">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="4139b-277">Una alternativa de la aplicación auxiliar HTML `Html.DropDownListFor` y`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="4139b-277">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="4139b-278">El `Select Tag Helper` `asp-for` especifica el nombre de la propiedad de modelo para el [seleccione](https://www.w3.org/wiki/HTML/Elements/select) elemento y `asp-items` especifica la [opción](https://www.w3.org/wiki/HTML/Elements/option) elementos.</span><span class="sxs-lookup"><span data-stu-id="4139b-278">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="4139b-279">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-279">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="4139b-280">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-280">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="4139b-281">El `Index` método inicializa la `CountryViewModel`, Establece el país seleccionado y lo pasa a la `Index` vista.</span><span class="sxs-lookup"><span data-stu-id="4139b-281">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="4139b-282">HTTP POST `Index` método muestra la selección:</span><span class="sxs-lookup"><span data-stu-id="4139b-282">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="4139b-283">El `Index` vista:</span><span class="sxs-lookup"><span data-stu-id="4139b-283">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="4139b-284">Que genera el siguiente código HTML (con "CA" seleccionada):</span><span class="sxs-lookup"><span data-stu-id="4139b-284">Which generates the following HTML (with "CA" selected):</span></span>

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
> <span data-ttu-id="4139b-285">Se recomienda no usar `ViewBag` o `ViewData` a la aplicación auxiliar seleccione etiqueta.</span><span class="sxs-lookup"><span data-stu-id="4139b-285">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="4139b-286">Un modelo de vista es más eficaz al proporcionar metadatos MVC y generalmente menos problemática.</span><span class="sxs-lookup"><span data-stu-id="4139b-286">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="4139b-287">El `asp-for` valor de atributo es un caso especial y no requiere un `Model` de prefijo, los otro no de atributos de etiqueta auxiliar (como `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="4139b-287">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="4139b-288">Enlace de enum</span><span class="sxs-lookup"><span data-stu-id="4139b-288">Enum binding</span></span>

<span data-ttu-id="4139b-289">A menudo resulta cómodo para utilizarlo `<select>` con una `enum` propiedad y generar el `SelectListItem` elementos desde el `enum` valores.</span><span class="sxs-lookup"><span data-stu-id="4139b-289">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="4139b-290">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4139b-290">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="4139b-291">El `GetEnumSelectList` método genera una `SelectList` objeto para una enumeración.</span><span class="sxs-lookup"><span data-stu-id="4139b-291">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="4139b-292">Puede decorar la lista de enumeradores con el `Display` atributo para obtener una interfaz de usuario más enriquecida:</span><span class="sxs-lookup"><span data-stu-id="4139b-292">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="4139b-293">Se genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-293">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="4139b-294">Grupo de opciones</span><span class="sxs-lookup"><span data-stu-id="4139b-294">Option Group</span></span>

<span data-ttu-id="4139b-295">El código HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento se genera cuando el modelo de vista contiene uno o varios `SelectListGroup` objetos.</span><span class="sxs-lookup"><span data-stu-id="4139b-295">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="4139b-296">El `CountryViewModelGroup` grupos el `SelectListItem` elementos en los grupos "América del Norte" y "Europe":</span><span class="sxs-lookup"><span data-stu-id="4139b-296">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="4139b-297">Los dos grupos se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="4139b-297">The two groups are shown below:</span></span>

![ejemplo de grupo de opción](working-with-forms/_static/grp.png)

<span data-ttu-id="4139b-299">El código HTML generado:</span><span class="sxs-lookup"><span data-stu-id="4139b-299">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="4139b-300">Selección múltiple</span><span class="sxs-lookup"><span data-stu-id="4139b-300">Multiple select</span></span>

<span data-ttu-id="4139b-301">La aplicación auxiliar seleccione etiqueta generará automáticamente la [varios = "varios"](http://w3c.github.io/html-reference/select.html) atributo si la propiedad especificada en el `asp-for` atributo es un `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="4139b-301">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="4139b-302">Por ejemplo, dado el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4139b-302">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="4139b-303">Con la siguiente vista:</span><span class="sxs-lookup"><span data-stu-id="4139b-303">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="4139b-304">Genera el siguiente código HTML:</span><span class="sxs-lookup"><span data-stu-id="4139b-304">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="4139b-305">No hay nada seleccionado</span><span class="sxs-lookup"><span data-stu-id="4139b-305">No selection</span></span>

<span data-ttu-id="4139b-306">Si se encuentra con la opción "no se ha especificado" en varias páginas, puede crear una plantilla para eliminar el código HTML de repetición:</span><span class="sxs-lookup"><span data-stu-id="4139b-306">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="4139b-307">El *Views/Shared/EditorTemplates/CountryViewModel.cshtml* plantilla:</span><span class="sxs-lookup"><span data-stu-id="4139b-307">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="4139b-308">Agregar HTML [ \<opción >](https://www.w3.org/wiki/HTML/Elements/option) elementos no se limita a la *ninguna selección* caso.</span><span class="sxs-lookup"><span data-stu-id="4139b-308">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="4139b-309">Por ejemplo, el método de acción y la vista siguiente generará HTML similar al código anterior:</span><span class="sxs-lookup"><span data-stu-id="4139b-309">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="4139b-310">El valor correcto `<option>` se seleccionará el elemento (contienen el `selected="selected"` atributo) según la actual `Country` valor.</span><span class="sxs-lookup"><span data-stu-id="4139b-310">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="4139b-311">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4139b-311">Additional Resources</span></span>

* [<span data-ttu-id="4139b-312">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="4139b-312">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="4139b-313">Elemento de formulario HTML</span><span class="sxs-lookup"><span data-stu-id="4139b-313">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="4139b-314">Solicitud de Token de comprobación</span><span class="sxs-lookup"><span data-stu-id="4139b-314">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="4139b-315">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="4139b-315">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="4139b-316">Validación del modelo</span><span class="sxs-lookup"><span data-stu-id="4139b-316">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="4139b-317">anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="4139b-317">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="4139b-318">[Fragmentos de este documento de código](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="4139b-318">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
