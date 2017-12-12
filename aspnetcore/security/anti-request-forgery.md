---
title: "Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core"
author: steve-smith
ms.author: riande
description: "Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: d7df8f91e88290509c8751a4b69804b60138846e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="7d633-103">Prevención de ataques de falsificación (XSRF/CSRF) de solicitud entre sitios en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d633-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="7d633-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d633-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="7d633-105">¿Qué ataque evita antifalsificación?</span><span class="sxs-lookup"><span data-stu-id="7d633-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="7d633-106">Falsificación de solicitudes entre sitios (también conocido como XSRF o CSRF, pronunciado *vea navegación*) es un ataque contra las aplicaciones hospedadas en web mediante el cual un sitio web malintencionado puede influir en la interacción entre un explorador del cliente y un sitio web que se confía ese explorador.</span><span class="sxs-lookup"><span data-stu-id="7d633-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="7d633-107">Estos ataques se realizan porque algunos tipos de tokens de autenticación automáticamente con cada solicitud de envío de los exploradores web a un sitio web.</span><span class="sxs-lookup"><span data-stu-id="7d633-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="7d633-108">Esta forma de vulnerabilidad de seguridad es también conocido como un *ataque de un solo clic* o como *sesión riding*, ya que el ataque se aprovecha del usuario del autenticado previamente la sesión.</span><span class="sxs-lookup"><span data-stu-id="7d633-108">This form of exploit is also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="7d633-109">Un ejemplo de un ataque CSRF:</span><span class="sxs-lookup"><span data-stu-id="7d633-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="7d633-110">Un usuario inicia sesión en `www.example.com`, utilizando la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="7d633-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="7d633-111">El servidor autentica al usuario y emite una respuesta que incluye una cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7d633-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="7d633-112">El usuario visita un sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="7d633-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="7d633-113">El sitio malintencionado contiene un formulario HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d633-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="7d633-114">Tenga en cuenta que la acción de formulario se envía al sitio vulnerable, no en el sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="7d633-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="7d633-115">Esta es la parte "cross-site" de CSRF.</span><span class="sxs-lookup"><span data-stu-id="7d633-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="7d633-116">El usuario hace clic en el botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="7d633-116">The user clicks the submit button.</span></span> <span data-ttu-id="7d633-117">El explorador incluye automáticamente la cookie de autenticación para el dominio solicitado (es decir, el sitio sea vulnerable en este caso) con la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d633-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="7d633-118">La solicitud se ejecuta en el servidor con el contexto de autenticación del usuario y puede hacer todo lo que puede hacer un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="7d633-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="7d633-119">Este ejemplo requiere que el usuario haga clic en el botón del formulario.</span><span class="sxs-lookup"><span data-stu-id="7d633-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="7d633-120">La página malintencionada podría:</span><span class="sxs-lookup"><span data-stu-id="7d633-120">The malicious page could:</span></span>

* <span data-ttu-id="7d633-121">Ejecutar un script que envía automáticamente el formulario.</span><span class="sxs-lookup"><span data-stu-id="7d633-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="7d633-122">Envía un envío de formulario como una solicitud AJAX.</span><span class="sxs-lookup"><span data-stu-id="7d633-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="7d633-123">Usar un formulario oculto con CSS.</span><span class="sxs-lookup"><span data-stu-id="7d633-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="7d633-124">Uso de SSL no evita que un ataque CSRF, el sitio malintencionado puede enviar un `https://` solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d633-124">Using SSL does not prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="7d633-125">Algunos ataques de destino los puntos de conexión de sitio que respondan a `GET` solicitudes, en cuyo caso una etiqueta de imagen puede se utiliza para realizar la acción (esta forma de ataque es común en los sitios de foro que permiten imágenes pero bloquean JavaScript).</span><span class="sxs-lookup"><span data-stu-id="7d633-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="7d633-126">Las aplicaciones que cambian el estado con `GET` solicitudes son vulnerables frente a ataques malintencionados.</span><span class="sxs-lookup"><span data-stu-id="7d633-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="7d633-127">Los ataques CSRF son posibles con sitios web que usan cookies para la autenticación, porque los exploradores envían todas las cookies relevantes para el sitio web de destino.</span><span class="sxs-lookup"><span data-stu-id="7d633-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="7d633-128">Sin embargo, los ataques CSRF no están limitados a aprovecharse de las cookies.</span><span class="sxs-lookup"><span data-stu-id="7d633-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="7d633-129">Por ejemplo, la autenticación básica e implícita también son vulnerables.</span><span class="sxs-lookup"><span data-stu-id="7d633-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="7d633-130">Una vez que un usuario inicia sesión con autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que finaliza la sesión.</span><span class="sxs-lookup"><span data-stu-id="7d633-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="7d633-131">Nota: en este contexto, *sesión* hace referencia a la sesión de cliente durante el cual el usuario está autenticado.</span><span class="sxs-lookup"><span data-stu-id="7d633-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="7d633-132">No está relacionado con las sesiones de servidor o [sesión middleware](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="7d633-132">It is unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="7d633-133">Los usuarios pueden protegerse contra vulnerabilidades CSRF por:</span><span class="sxs-lookup"><span data-stu-id="7d633-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="7d633-134">Registro fuera de sitios web cuando termine de utilizarlos.</span><span class="sxs-lookup"><span data-stu-id="7d633-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="7d633-135">Borrar las cookies de su explorador periódicamente.</span><span class="sxs-lookup"><span data-stu-id="7d633-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="7d633-136">Sin embargo, las vulnerabilidades CSRF son fundamentalmente un problema con la aplicación web, no el usuario final.</span><span class="sxs-lookup"><span data-stu-id="7d633-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="7d633-137">¿Cómo satisface MVC de ASP.NET Core CSRF?</span><span class="sxs-lookup"><span data-stu-id="7d633-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="7d633-138">ASP.NET Core implementa anti-la falsificación de solicitud usando el [pila de protección de datos de ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="7d633-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="7d633-139">Protección de datos de ASP.NET Core debe configurarse para que funcione en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="7d633-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="7d633-140">Vea [configurando la protección de datos](xref:security/data-protection/configuration/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7d633-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="7d633-141">Configuración de protección de datos de ASP.NET Core anti-la falsificación de solicitud predeterminado</span><span class="sxs-lookup"><span data-stu-id="7d633-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="7d633-142">En el núcleo de ASP.NET MVC 2.0 la [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserta tokens antifalsificación para los elementos de formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="7d633-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="7d633-143">Por ejemplo, el marcado siguiente en un archivo Razor generará automáticamente tokens antifalsificación:</span><span class="sxs-lookup"><span data-stu-id="7d633-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="7d633-144">La generación automática de tokens antifalsificación para los elementos de formulario HTML ocurre cuando:</span><span class="sxs-lookup"><span data-stu-id="7d633-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="7d633-145">El `form` etiqueta contiene el `method="post"` atributo AND</span><span class="sxs-lookup"><span data-stu-id="7d633-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="7d633-146">El atributo de acción está vacío.</span><span class="sxs-lookup"><span data-stu-id="7d633-146">The action attribute is empty.</span></span> <span data-ttu-id="7d633-147">( `action=""`) O</span><span class="sxs-lookup"><span data-stu-id="7d633-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="7d633-148">No se proporcionó el atributo de acción.</span><span class="sxs-lookup"><span data-stu-id="7d633-148">The action attribute is not supplied.</span></span> <span data-ttu-id="7d633-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="7d633-149">(`<form method="post">`)</span></span>

<span data-ttu-id="7d633-150">Puede deshabilitar la generación automática de tokens antifalsificación para los elementos de formulario HTML mediante:</span><span class="sxs-lookup"><span data-stu-id="7d633-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="7d633-151">Deshabilita explícitamente `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="7d633-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="7d633-152">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="7d633-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="7d633-153">Elegir el elemento de formulario de aplicaciones auxiliares de etiquetas mediante el uso de la aplicación auxiliar de etiqueta [! desactivación símbolo](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="7d633-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="7d633-154">Quitar el `FormTagHelper` de la vista.</span><span class="sxs-lookup"><span data-stu-id="7d633-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="7d633-155">Puede quitar el `FormTagHelper` desde una vista agregando la siguiente directiva a la vista Razor:</span><span class="sxs-lookup"><span data-stu-id="7d633-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="7d633-156">[Las páginas de Razor](xref:mvc/razor-pages/index) están protegidos automáticamente frente a XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="7d633-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="7d633-157">No tienes que escribir ningún código adicional.</span><span class="sxs-lookup"><span data-stu-id="7d633-157">You don't have to write any additional code.</span></span> <span data-ttu-id="7d633-158">Vea [XSRF/CSRF y páginas de Razor](xref:mvc/razor-pages/index#xsrf) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7d633-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="7d633-159">El enfoque más común para defenderse contra ataques CSRF es el patrón del token Sincronizador (STP).</span><span class="sxs-lookup"><span data-stu-id="7d633-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="7d633-160">STP es una técnica que se utiliza cuando el usuario solicita una página con datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="7d633-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="7d633-161">El servidor envía un token asociado con la identidad del usuario actual al cliente.</span><span class="sxs-lookup"><span data-stu-id="7d633-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="7d633-162">El cliente devuelve el token en el servidor para la comprobación.</span><span class="sxs-lookup"><span data-stu-id="7d633-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="7d633-163">Si el servidor recibe un token que no coincide con la identidad del usuario autenticado, se rechaza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d633-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="7d633-164">El token es único y no previstos.</span><span class="sxs-lookup"><span data-stu-id="7d633-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="7d633-165">El token también puede usarse para asegurarse de la secuencia correcta de una serie de solicitudes (página 2 que precede a la página 3 que precede a garantizar página 1).</span><span class="sxs-lookup"><span data-stu-id="7d633-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="7d633-166">Todas las formas en las plantillas de MVC de ASP.NET Core generan tokens antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7d633-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="7d633-167">Los dos ejemplos siguientes de lógica de vista generan tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="7d633-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="7d633-168">Puede agregar explícitamente un token antiforgery a una ``<form>`` elemento sin usar aplicaciones auxiliares de etiquetas con la aplicación auxiliar HTML ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="7d633-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="7d633-169">En cada uno de los casos anteriores, ASP.NET Core se agregará un campo de formulario oculto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d633-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="7d633-170">ASP.NET Core incluye tres [filtros](xref:mvc/controllers/filters) para trabajar con tokens antiforgery: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, y ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="7d633-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="7d633-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="7d633-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="7d633-172">El ``ValidateAntiForgeryToken`` es un filtro de acción que se puede aplicar a una acción individual, un controlador, o de forma global.</span><span class="sxs-lookup"><span data-stu-id="7d633-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="7d633-173">Se bloquearán las solicitudes realizadas a las acciones que tienen este filtro se aplica a menos que la solicitud incluye un token de antiforgery válido.</span><span class="sxs-lookup"><span data-stu-id="7d633-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="7d633-174">El ``ValidateAntiForgeryToken`` atributo requiere un token para las solicitudes a los métodos de acción decora, incluidos `HTTP GET` solicitudes.</span><span class="sxs-lookup"><span data-stu-id="7d633-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="7d633-175">Si se aplica ampliamente, puede reemplazarlo con el ``IgnoreAntiforgeryToken`` atributo.</span><span class="sxs-lookup"><span data-stu-id="7d633-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="7d633-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7d633-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="7d633-177">Por lo general, las aplicaciones de ASP.NET Core no generan antiforgery tokens para los métodos de prueba de errores de HTTP (GET, HEAD, opciones y seguimiento).</span><span class="sxs-lookup"><span data-stu-id="7d633-177">ASP.NET Core apps generally do not generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="7d633-178">En lugar de aplicar ampliamente el ``ValidateAntiForgeryToken`` atributo y, a continuación, reemplazar con ``IgnoreAntiforgeryToken`` atributos, puede utilizar el ``AutoValidateAntiforgeryToken`` atributo.</span><span class="sxs-lookup"><span data-stu-id="7d633-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="7d633-179">Este atributo funciona de forma idéntica a la ``ValidateAntiForgeryToken`` de atributo, salvo que no requiere tokens para las solicitudes realizadas con los siguientes métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="7d633-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="7d633-180">GET</span><span class="sxs-lookup"><span data-stu-id="7d633-180">GET</span></span>
* <span data-ttu-id="7d633-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="7d633-181">HEAD</span></span>
* <span data-ttu-id="7d633-182">OPCIONES</span><span class="sxs-lookup"><span data-stu-id="7d633-182">OPTIONS</span></span>
* <span data-ttu-id="7d633-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="7d633-183">TRACE</span></span>

<span data-ttu-id="7d633-184">Le recomendamos que use ``AutoValidateAntiforgeryToken`` ampliamente para escenarios de API no.</span><span class="sxs-lookup"><span data-stu-id="7d633-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="7d633-185">Esto garantiza que las acciones de entrada están protegidas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7d633-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="7d633-186">La alternativa es omitir antiforgery símbolos (tokens) de forma predeterminada, a menos que ``ValidateAntiForgeryToken`` se aplica al método de acción individuales.</span><span class="sxs-lookup"><span data-stu-id="7d633-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="7d633-187">Es más probable en este escenario para un método de acción de POST como izquierda sin protección, salir de la aplicación sea vulnerable a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="7d633-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="7d633-188">ENTRADAS incluso anónimos deben enviar el token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7d633-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="7d633-189">Nota: Las API no tienen un mecanismo automático para el envío de la parte no cookie del token; es probable que dependerán de su implementación en la implementación de código de cliente.</span><span class="sxs-lookup"><span data-stu-id="7d633-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="7d633-190">A continuación se muestran algunos ejemplos.</span><span class="sxs-lookup"><span data-stu-id="7d633-190">Some examples are shown below.</span></span>


<span data-ttu-id="7d633-191">Ejemplo (clase):</span><span class="sxs-lookup"><span data-stu-id="7d633-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="7d633-192">Ejemplo (global):</span><span class="sxs-lookup"><span data-stu-id="7d633-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="7d633-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="7d633-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="7d633-194">El ``IgnoreAntiforgeryToken`` filtro se utiliza para eliminar la necesidad de un token antiforgery estén presentes para una acción determinada (o controlador).</span><span class="sxs-lookup"><span data-stu-id="7d633-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="7d633-195">Cuando se aplica, reemplazará este filtro ``ValidateAntiForgeryToken`` o ``AutoValidateAntiforgeryToken`` filtros especificados en un nivel más alto (global o en un controlador).</span><span class="sxs-lookup"><span data-stu-id="7d633-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="7d633-196">JavaScript, AJAX y SPAs</span><span class="sxs-lookup"><span data-stu-id="7d633-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="7d633-197">En las aplicaciones tradicionales basadas en HTML, antiforgery símbolos (tokens) se pasa al servidor mediante campos ocultos de formulario.</span><span class="sxs-lookup"><span data-stu-id="7d633-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="7d633-198">En las aplicaciones modernas basadas en JavaScript y aplicaciones de una página (SPAs), muchas de las solicitudes se realizan mediante programación.</span><span class="sxs-lookup"><span data-stu-id="7d633-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="7d633-199">Estas solicitudes de AJAX pueden usar otras técnicas (por ejemplo, los encabezados de solicitud o cookies) para enviar el token.</span><span class="sxs-lookup"><span data-stu-id="7d633-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="7d633-200">Si se usan cookies para almacenar los tokens de autenticación y para autenticar las solicitudes de API en el servidor, CSRF será un problema potencial.</span><span class="sxs-lookup"><span data-stu-id="7d633-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="7d633-201">Sin embargo, si se usa almacenamiento local para almacenar el token, CSRF vulnerabilidad puede ser mitigada, puesto que no se envían automáticamente los valores desde el almacenamiento local en el servidor con cada nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d633-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="7d633-202">Por lo tanto, puede utilizar almacenamiento local para almacenar el token antiforgery en el cliente y enviar el token como un encabezado de solicitud es un enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="7d633-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="7d633-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="7d633-203">AngularJS</span></span>

<span data-ttu-id="7d633-204">AngularJS usa una convención para dirección CSRF.</span><span class="sxs-lookup"><span data-stu-id="7d633-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="7d633-205">Si el servidor envía una cookie con el nombre ``XSRF-TOKEN``, el Angular ``$http`` servicio agregará el valor de esta cookie a un encabezado cuando envía una solicitud a este servidor.</span><span class="sxs-lookup"><span data-stu-id="7d633-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="7d633-206">Este proceso es automático; no es necesario establecer explícitamente el encabezado.</span><span class="sxs-lookup"><span data-stu-id="7d633-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="7d633-207">El nombre de encabezado es ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="7d633-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="7d633-208">El servidor debe detectar este encabezado y validar su contenido.</span><span class="sxs-lookup"><span data-stu-id="7d633-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="7d633-209">Para la API de ASP.NET básica trabajar con esta convención:</span><span class="sxs-lookup"><span data-stu-id="7d633-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="7d633-210">Configurar la aplicación para proporcionar un token en una cookie denominada``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="7d633-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="7d633-211">Configurar el servicio para que busque un encabezado denominado antiforgery``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="7d633-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="7d633-212">[Ejemplo de vista](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="7d633-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="7d633-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7d633-213">JavaScript</span></span>

<span data-ttu-id="7d633-214">Uso de JavaScript con vistas, puede crear el token con un servicio en la vista de.</span><span class="sxs-lookup"><span data-stu-id="7d633-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="7d633-215">Para ello, insertar el `Microsoft.AspNetCore.Antiforgery.IAntiforgery` servicio en la vista y llame a `GetAndStoreTokens`, como se muestra:</span><span class="sxs-lookup"><span data-stu-id="7d633-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="7d633-216">Este enfoque elimina la necesidad de tratar directamente con la configuración de cookies del servidor o para leerlos desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="7d633-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="7d633-217">JavaScript puede tener acceso a tokens que proporcionó en las cookies y, a continuación, usar el contenido de la cookie para crear un encabezado con el valor del token, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7d633-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="7d633-218">A continuación, suponiendo que crear su script solicita para enviar el token en un encabezado denominado ``X-CSRF-TOKEN``, configurar el servicio antiforgery para buscar la ``X-CSRF-TOKEN`` encabezado:</span><span class="sxs-lookup"><span data-stu-id="7d633-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="7d633-219">En el ejemplo siguiente se utiliza jQuery para realizar una solicitud de AJAX con el encabezado adecuado:</span><span class="sxs-lookup"><span data-stu-id="7d633-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="7d633-220">Configurar Antiforgery</span><span class="sxs-lookup"><span data-stu-id="7d633-220">Configuring Antiforgery</span></span>

<span data-ttu-id="7d633-221">`IAntiforgery`proporciona la API para configurar el sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7d633-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="7d633-222">Se puede solicitar en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="7d633-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="7d633-223">En el ejemplo siguiente se usa el middleware de página principal de la aplicación para generar un token de antiforgery y enviarlo en la respuesta como una cookie (mediante la convención de nomenclatura de manera predeterminada Angular que se ha descrito anteriormente):</span><span class="sxs-lookup"><span data-stu-id="7d633-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="7d633-224">Opciones</span><span class="sxs-lookup"><span data-stu-id="7d633-224">Options</span></span>

<span data-ttu-id="7d633-225">Puede personalizar [opciones antiforgery](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7d633-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="7d633-226">Opción</span><span class="sxs-lookup"><span data-stu-id="7d633-226">Option</span></span>        | <span data-ttu-id="7d633-227">Descripción</span><span class="sxs-lookup"><span data-stu-id="7d633-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="7d633-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="7d633-228">CookieDomain</span></span>  | <span data-ttu-id="7d633-229">El dominio de la cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-229">The domain of the cookie.</span></span> <span data-ttu-id="7d633-230">Tiene como valor predeterminado `null`.</span><span class="sxs-lookup"><span data-stu-id="7d633-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="7d633-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="7d633-231">CookieName</span></span>    | <span data-ttu-id="7d633-232">El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-232">The name of the cookie.</span></span> <span data-ttu-id="7d633-233">Si no se establece, el sistema genera un nombre único que comienza con la `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="7d633-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="7d633-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="7d633-234">CookiePath</span></span>    | <span data-ttu-id="7d633-235">La ruta de acceso establecido en la cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="7d633-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="7d633-236">FormFieldName</span></span> | <span data-ttu-id="7d633-237">El nombre del campo oculto del formulario utilizado por el sistema antiforgery para representar antiforgery tokens en las vistas.</span><span class="sxs-lookup"><span data-stu-id="7d633-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="7d633-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="7d633-238">HeaderName</span></span>    | <span data-ttu-id="7d633-239">El nombre del encabezado utilizado por el sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7d633-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="7d633-240">Si `null`, el sistema considerará únicamente datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="7d633-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="7d633-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="7d633-241">RequireSsl</span></span>    | <span data-ttu-id="7d633-242">Especifica si se requiere SSL por el sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="7d633-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="7d633-243">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="7d633-243">Defaults to `false`.</span></span> <span data-ttu-id="7d633-244">Si `true`, se producirá un error en las solicitudes sin SSL.</span><span class="sxs-lookup"><span data-stu-id="7d633-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="7d633-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="7d633-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="7d633-246">Especifica si se debe suprimir la generación de la `X-Frame-Options` encabezado.</span><span class="sxs-lookup"><span data-stu-id="7d633-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="7d633-247">De forma predeterminada, el encabezado se genera con un valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="7d633-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="7d633-248">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="7d633-248">Defaults to `false`.</span></span> |

<span data-ttu-id="7d633-249">Consulte https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7d633-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="7d633-250">Extender Antiforgery</span><span class="sxs-lookup"><span data-stu-id="7d633-250">Extending Antiforgery</span></span>

<span data-ttu-id="7d633-251">El [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite a los desarrolladores extender el comportamiento del sistema anti-XSRF por los datos adicionales de ida y vuelta de cada token.</span><span class="sxs-lookup"><span data-stu-id="7d633-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="7d633-252">El [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado.</span><span class="sxs-lookup"><span data-stu-id="7d633-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="7d633-253">Un implementador podría devolver una marca de tiempo, un nonce o cualquier otro valor y, a continuación, llame a [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) para validar estos datos cuando se valida el token.</span><span class="sxs-lookup"><span data-stu-id="7d633-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="7d633-254">Nombre de usuario del cliente ya está incrustada en los tokens generados, por lo que no es necesario incluir esta información.</span><span class="sxs-lookup"><span data-stu-id="7d633-254">The client's username is already embedded in the generated tokens, so there is no need to include this information.</span></span> <span data-ttu-id="7d633-255">Si un token incluye datos suplementarios pero no `IAntiForgeryAdditionalDataProvider` ha sido configurado, los datos suplementarios no se validan.</span><span class="sxs-lookup"><span data-stu-id="7d633-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data is not validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="7d633-256">Aspectos básicos</span><span class="sxs-lookup"><span data-stu-id="7d633-256">Fundamentals</span></span>

<span data-ttu-id="7d633-257">Ataques CSRF se basan en el comportamiento del explorador predeterminado de enviar cookies asociadas a un dominio con cada solicitud realizada a ese dominio.</span><span class="sxs-lookup"><span data-stu-id="7d633-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="7d633-258">Estas cookies se almacenan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="7d633-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="7d633-259">Con frecuencia incluyen cookies de sesión para los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="7d633-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="7d633-260">Autenticación basada en cookies es una forma popular de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7d633-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="7d633-261">Sistemas de autenticación basada en autorización token han ido aumentando popular, especialmente para SPAs y otros escenarios "smart client".</span><span class="sxs-lookup"><span data-stu-id="7d633-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="7d633-262">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="7d633-262">Cookie-based authentication</span></span>

<span data-ttu-id="7d633-263">Una vez que un usuario se autentica con su nombre de usuario y contraseña, se emiten un símbolo (token) que puede usarse para identificarlos y validar que se han autenticado.</span><span class="sxs-lookup"><span data-stu-id="7d633-263">Once a user has authenticated using their username and password, they are issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="7d633-264">El token se almacena como hace que una cookie que acompaña a cada solicitud del cliente.</span><span class="sxs-lookup"><span data-stu-id="7d633-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="7d633-265">Generar y validar esta cookie se realiza mediante el middleware de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="7d633-266">ASP.NET Core proporciona una cookie [middleware](../fundamentals/middleware.md) que serializa una entidad de seguridad de usuario en una cookie cifrada y, a continuación, en solicitudes posteriores, valida la cookie, vuelve a crear la entidad de seguridad y lo asigna a la `User` propiedad `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7d633-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="7d633-267">Cuando se utiliza una cookie, la cookie de autenticación es simplemente un contenedor para el vale de autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="7d633-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="7d633-268">El vale se pasa como el valor de la cookie de autenticación de formularios con cada solicitud y autenticación de formularios, en el servidor, se usa para identificar un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="7d633-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="7d633-269">Cuando un usuario inicia sesión un sistema, una sesión de usuario se crea en el servidor y se almacena en una base de datos u otro almacén persistente.</span><span class="sxs-lookup"><span data-stu-id="7d633-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="7d633-270">El sistema genera una clave de sesión que hacen referencia a la sesión actual en el almacén de datos y se envía como una cookie del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7d633-270">The system generates a session key that points to the actual session in the data store and it is sent as a client side cookie.</span></span> <span data-ttu-id="7d633-271">El servidor web comprobará esta clave de sesión siempre que un usuario solicita un recurso que requiera una autorización.</span><span class="sxs-lookup"><span data-stu-id="7d633-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="7d633-272">El sistema comprueba si la sesión de usuario asociado tiene el privilegio de acceso al recurso solicitado.</span><span class="sxs-lookup"><span data-stu-id="7d633-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="7d633-273">Si es así, se continúa la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7d633-273">If so, the request continues.</span></span> <span data-ttu-id="7d633-274">En caso contrario, la solicitud se devuelve como no autorizados.</span><span class="sxs-lookup"><span data-stu-id="7d633-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="7d633-275">En este enfoque, las cookies se utilizan para hacer que la aplicación parece ser con estado, ya que es capaz de "recordar" el usuario ha autenticado previamente con el servidor.</span><span class="sxs-lookup"><span data-stu-id="7d633-275">In this approach, cookies are used to make the application appear to be stateful, since it is able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="7d633-276">Tokens de usuario</span><span class="sxs-lookup"><span data-stu-id="7d633-276">User tokens</span></span>

<span data-ttu-id="7d633-277">Autenticación basada en token no almacena la sesión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7d633-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="7d633-278">En su lugar, cuando un usuario ha iniciado sesión se emiten un token (no un token antiforgery).</span><span class="sxs-lookup"><span data-stu-id="7d633-278">Instead, when a user is logged in they are issued a token (not an antiforgery token).</span></span> <span data-ttu-id="7d633-279">Este token contiene todos los datos que es necesario para validar el token.</span><span class="sxs-lookup"><span data-stu-id="7d633-279">This token holds all the data that is required to validate the token.</span></span> <span data-ttu-id="7d633-280">También contiene información de usuario, en forma de [notificaciones](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="7d633-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="7d633-281">Cuando un usuario desea tener acceso a un recurso de servidor que requiera autenticación, el token se envía al servidor con un encabezado de autorización adicionales en forma de portador {token}.</span><span class="sxs-lookup"><span data-stu-id="7d633-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="7d633-282">Esto hace que la aplicación sin estado ya que en cada solicitud posterior se pasa el token de la solicitud de validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="7d633-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="7d633-283">Este token no es *cifrados*; en su lugar es *codificado*.</span><span class="sxs-lookup"><span data-stu-id="7d633-283">This token is not *encrypted*; rather it is *encoded*.</span></span> <span data-ttu-id="7d633-284">En el lado del servidor puede descodificar el token para tener acceso a la información sin procesar en el token.</span><span class="sxs-lookup"><span data-stu-id="7d633-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="7d633-285">Para enviar el token en solicitudes posteriores, o bien puede almacenarlo en el almacenamiento local del explorador o en una cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="7d633-286">No tiene que preocuparse de vulnerabilidad XSRF si el token se almacena en el almacenamiento local, pero es un problema si el token está almacenado en una cookie.</span><span class="sxs-lookup"><span data-stu-id="7d633-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it is an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="7d633-287">Varias aplicaciones se hospedan en un dominio</span><span class="sxs-lookup"><span data-stu-id="7d633-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="7d633-288">Aunque `example1.cloudapp.net` y `example2.cloudapp.net` son diferentes de los hosts, hay una relación de confianza implícita entre todos los hosts bajo la `*.cloudapp.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="7d633-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there is an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="7d633-289">Esta relación de confianza implícita permite a los hosts no sea de confianza influir en sus respectivas cookies (las directivas de mismo origen que rigen las solicitudes AJAX no necesariamente se aplican a las cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="7d633-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests do not necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="7d633-290">El tiempo de ejecución de ASP.NET Core proporciona algunos mitigación en que el nombre de usuario se incrusta en el símbolo (token) de campo, por lo que incluso si un subdominio malintencionado puede sobrescribir un token de sesión no podrá generar un token de campo válido para el usuario.</span><span class="sxs-lookup"><span data-stu-id="7d633-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="7d633-291">Sin embargo, cuando se hospeda en un entorno de este tipo las rutinas de anti-XSRF integrado aún no defienden contra secuestro de sesión o inicio de sesión CSRF ataques.</span><span class="sxs-lookup"><span data-stu-id="7d633-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="7d633-292">Entornos de hospedaje compartidos son vulnerable a secuestro de sesión, inicio de sesión CSRF y otros ataques.</span><span class="sxs-lookup"><span data-stu-id="7d633-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="7d633-293">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7d633-293">Additional Resources</span></span>

* <span data-ttu-id="7d633-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) en [Abrir proyecto de seguridad de aplicación Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="7d633-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
