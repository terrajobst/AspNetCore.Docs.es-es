---
title: Ataques de evitar Cross-Site falsificación de solicitud (XSRF/CSRF) en ASP.NET Core
author: steve-smith
description: Descubra cómo impedir ataques contra las aplicaciones web en un sitio Web malintencionado puede influir en la interacción entre un explorador del cliente y la aplicación.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="961f9-103">Ataques de evitar Cross-Site falsificación de solicitud (XSRF/CSRF) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="961f9-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="961f9-104">Por [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="961f9-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="961f9-105">Falsificación de solicitudes entre sitios (también conocido como XSRF o CSRF, pronunciado *vea navegación*) es un ataque contra mediante el cual una aplicación web malintencionado puede influir en la interacción entre un explorador del cliente y una aplicación web que confía en que las aplicaciones hospedadas en web Explorador.</span><span class="sxs-lookup"><span data-stu-id="961f9-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="961f9-106">Estos ataques son posibles porque los exploradores web envían algunos tipos de tokens de autenticación automáticamente con cada solicitud a un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="961f9-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="961f9-107">Esta forma de vulnerabilidad de seguridad es también conocido como un *ataque de un solo clic* o *sesión conducir* porque el ataque aprovecha las ventajas de sesión del autentica previamente en el usuario.</span><span class="sxs-lookup"><span data-stu-id="961f9-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="961f9-108">Un ejemplo de un ataque CSRF:</span><span class="sxs-lookup"><span data-stu-id="961f9-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="961f9-109">Un usuario inicia sesión en `www.good-banking-site.com` utilizando la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="961f9-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="961f9-110">El servidor autentica al usuario y emite una respuesta que incluye una cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="961f9-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="961f9-111">El sitio es vulnerable a ataques porque confía en cualquier solicitud recibida con una cookie de autenticación válida.</span><span class="sxs-lookup"><span data-stu-id="961f9-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="961f9-112">El usuario visita un sitio malintencionado, `www.bad-crook-site.com`.</span><span class="sxs-lookup"><span data-stu-id="961f9-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="961f9-113">El sitio malintencionado, `www.bad-crook-site.com`, contiene un formulario HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="961f9-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="961f9-114">Tenga en cuenta que el formulario `action` entradas para el sitio sea vulnerable, no para el sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="961f9-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="961f9-115">Esta es la parte "cross-site" de CSRF.</span><span class="sxs-lookup"><span data-stu-id="961f9-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="961f9-116">El usuario selecciona el botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="961f9-116">The user selects the submit button.</span></span> <span data-ttu-id="961f9-117">El explorador realiza la solicitud y automáticamente incluye la cookie de autenticación para el dominio solicitado, `www.good-banking-site.com`.</span><span class="sxs-lookup"><span data-stu-id="961f9-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="961f9-118">La solicitud se ejecuta en el `www.good-banking-site.com` servidor con contexto de autenticación del usuario y pueden realizar cualquier acción que puede realizar un usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="961f9-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="961f9-119">Cuando el usuario selecciona el botón para enviar el formulario, el sitio malintencionado podría:</span><span class="sxs-lookup"><span data-stu-id="961f9-119">When the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="961f9-120">Ejecutar un script que envía automáticamente el formulario.</span><span class="sxs-lookup"><span data-stu-id="961f9-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="961f9-121">Envía un envío de formulario como una solicitud AJAX.</span><span class="sxs-lookup"><span data-stu-id="961f9-121">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="961f9-122">Usar un formulario oculto con CSS.</span><span class="sxs-lookup"><span data-stu-id="961f9-122">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="961f9-123">Uso de HTTPS no impide que un ataque CSRF.</span><span class="sxs-lookup"><span data-stu-id="961f9-123">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="961f9-124">El sitio malintencionado puede enviar un `https://www.good-banking-site.com/` solicitar de manera tan sencilla como puede enviar una solicitud insegura.</span><span class="sxs-lookup"><span data-stu-id="961f9-124">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="961f9-125">Algunos ataques de destino que respondan a las solicitudes GET, en cuyo caso se puede utilizar una etiqueta de imagen para realizar la acción.</span><span class="sxs-lookup"><span data-stu-id="961f9-125">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="961f9-126">Esta forma de ataque es habitual en los sitios de foro que permiten imágenes pero bloquean JavaScript.</span><span class="sxs-lookup"><span data-stu-id="961f9-126">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="961f9-127">Las aplicaciones que cambian el estado de las solicitudes GET, donde se modifican las variables o los recursos, son vulnerables a ataques malintencionados.</span><span class="sxs-lookup"><span data-stu-id="961f9-127">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="961f9-128">**Las solicitudes GET que cambiarán el estado no son seguros. Una práctica recomendada consiste en no cambiar nunca el estado en una solicitud GET.**</span><span class="sxs-lookup"><span data-stu-id="961f9-128">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="961f9-129">Los ataques CSRF son posibles con aplicaciones web que usan cookies para la autenticación porque:</span><span class="sxs-lookup"><span data-stu-id="961f9-129">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="961f9-130">Los exploradores almacenan las cookies emitidas por una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="961f9-130">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="961f9-131">Almacenado cookies incluyen cookies de sesión para los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="961f9-131">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="961f9-132">Exploradores envían que todas las cookies asociadas con un dominio a la aplicación web de todas las solicitudes, independientemente de cómo se generó la solicitud de aplicación dentro del explorador.</span><span class="sxs-lookup"><span data-stu-id="961f9-132">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="961f9-133">Sin embargo, no están limitados CSRF ataques para aprovecharse de las cookies.</span><span class="sxs-lookup"><span data-stu-id="961f9-133">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="961f9-134">Por ejemplo, la autenticación básica e implícita también son vulnerables.</span><span class="sxs-lookup"><span data-stu-id="961f9-134">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="961f9-135">Una vez que un usuario inicia sesión con autenticación básica o implícita, el explorador envía automáticamente las credenciales hasta que la sesión&dagger; finaliza.</span><span class="sxs-lookup"><span data-stu-id="961f9-135">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="961f9-136">&dagger;En este contexto, *sesión* hace referencia a la sesión de cliente durante el cual el usuario está autenticado.</span><span class="sxs-lookup"><span data-stu-id="961f9-136">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="961f9-137">Es no relacionados con las sesiones de servidor o [Middleware de sesión de ASP.NET Core](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="961f9-137">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="961f9-138">Los usuarios pueden protegerse contra vulnerabilidades CSRF, tome precauciones:</span><span class="sxs-lookup"><span data-stu-id="961f9-138">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="961f9-139">Inicie sesión en las aplicaciones web cuando termine de utilizarlos.</span><span class="sxs-lookup"><span data-stu-id="961f9-139">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="961f9-140">Borrar las cookies del explorador periódicamente.</span><span class="sxs-lookup"><span data-stu-id="961f9-140">Clear browser cookies periodically.</span></span>

<span data-ttu-id="961f9-141">Sin embargo, las vulnerabilidades CSRF son fundamentalmente un problema con la aplicación web, no el usuario final.</span><span class="sxs-lookup"><span data-stu-id="961f9-141">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="961f9-142">Conceptos básicos de autenticación</span><span class="sxs-lookup"><span data-stu-id="961f9-142">Authentication fundamentals</span></span>

<span data-ttu-id="961f9-143">Autenticación basada en cookies es una forma popular de autenticación.</span><span class="sxs-lookup"><span data-stu-id="961f9-143">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="961f9-144">Sistemas de autenticación basada en token son cada vez más popularidad, especialmente para las aplicaciones de página única (SPAs).</span><span class="sxs-lookup"><span data-stu-id="961f9-144">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="961f9-145">Autenticación basada en cookies</span><span class="sxs-lookup"><span data-stu-id="961f9-145">Cookie-based authentication</span></span>

<span data-ttu-id="961f9-146">Cuando un usuario se autentica con su nombre de usuario y contraseña, le emite un token, que contiene un vale de autenticación que se puede usar para la autenticación y autorización.</span><span class="sxs-lookup"><span data-stu-id="961f9-146">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="961f9-147">El token se almacena como hace que una cookie que acompaña a cada solicitud del cliente.</span><span class="sxs-lookup"><span data-stu-id="961f9-147">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="961f9-148">Generar y validar esta cookie se realizan mediante el Middleware de autenticación de la Cookie.</span><span class="sxs-lookup"><span data-stu-id="961f9-148">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="961f9-149">El [middleware](xref:fundamentals/middleware/index) serializa una entidad de seguridad de usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="961f9-149">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="961f9-150">En solicitudes posteriores, el middleware valida la cookie, vuelve a crear la entidad de seguridad y asigna la entidad de seguridad para la [usuario](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) propiedad de [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="961f9-150">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="961f9-151">Autenticación basada en token</span><span class="sxs-lookup"><span data-stu-id="961f9-151">Token-based authentication</span></span>

<span data-ttu-id="961f9-152">Cuando un usuario se autentica, le emite un token (no un token antiforgery).</span><span class="sxs-lookup"><span data-stu-id="961f9-152">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="961f9-153">El token contiene información de usuario en forma de [notificaciones](/dotnet/framework/security/claims-based-identity-model) o un símbolo (token) de referencia que señala la aplicación al estado de usuario que se mantienen en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="961f9-153">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="961f9-154">Cuando un usuario intenta acceder a un recurso que requiere autenticación, el token se envía a la aplicación con un encabezado de autorización adicionales en forma de token de portador.</span><span class="sxs-lookup"><span data-stu-id="961f9-154">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="961f9-155">Esto hace que la aplicación sin estado.</span><span class="sxs-lookup"><span data-stu-id="961f9-155">This makes the app stateless.</span></span> <span data-ttu-id="961f9-156">En cada solicitud posterior, el token se pasa en la solicitud para la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="961f9-156">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="961f9-157">Este token no es *cifrados*; tiene *codificado*.</span><span class="sxs-lookup"><span data-stu-id="961f9-157">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="961f9-158">En el servidor, se descodifica el token para acceder a su información.</span><span class="sxs-lookup"><span data-stu-id="961f9-158">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="961f9-159">Para enviar el token en las solicitudes subsiguientes, almacene el token en el almacenamiento local del explorador.</span><span class="sxs-lookup"><span data-stu-id="961f9-159">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="961f9-160">No se preocupe acerca de vulnerabilidad CSRF si el token se almacena en almacenamiento local del explorador.</span><span class="sxs-lookup"><span data-stu-id="961f9-160">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="961f9-161">CSRF es una preocupación cuando el token se almacena en una cookie.</span><span class="sxs-lookup"><span data-stu-id="961f9-161">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="961f9-162">Varias aplicaciones hospedadas en un dominio</span><span class="sxs-lookup"><span data-stu-id="961f9-162">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="961f9-163">Entornos de hospedaje compartidos son vulnerables a secuestro de sesión, inicio de sesión CSRF y otros ataques.</span><span class="sxs-lookup"><span data-stu-id="961f9-163">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="961f9-164">Aunque `example1.contoso.net` y `example2.contoso.net` son diferentes de los hosts, hay una relación de confianza implícita entre los hosts bajo la `*.contoso.net` dominio.</span><span class="sxs-lookup"><span data-stu-id="961f9-164">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="961f9-165">Esta relación de confianza implícita permite a los hosts no sea de confianza influir en sus respectivas cookies (las directivas de mismo origen que rigen las solicitudes AJAX necesariamente no se aplican a las cookies HTTP).</span><span class="sxs-lookup"><span data-stu-id="961f9-165">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="961f9-166">Se pueden evitar ataques que aprovechan las cookies de confianza entre las aplicaciones hospedadas en el mismo dominio, como no compartir dominios.</span><span class="sxs-lookup"><span data-stu-id="961f9-166">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="961f9-167">Cuando cada aplicación se hospeda en su propio dominio, no hay ninguna relación de confianza implícita cookie aprovechar.</span><span class="sxs-lookup"><span data-stu-id="961f9-167">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="961f9-168">Configuración de ASP.NET Core antiforgery</span><span class="sxs-lookup"><span data-stu-id="961f9-168">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="961f9-169">ASP.NET Core implementa con antiforgery [protección de datos de ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="961f9-169">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="961f9-170">La pila de protección de datos debe configurarse para que funcione en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="961f9-170">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="961f9-171">Vea [configurando la protección de datos](xref:security/data-protection/configuration/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="961f9-171">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="961f9-172">En el núcleo de ASP.NET 2.0 o posterior, el [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inyecta antiforgery tokens en elementos de formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="961f9-172">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="961f9-173">El siguiente marcado en un archivo Razor automáticamente genera tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="961f9-173">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="961f9-174">De forma similar, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera antiforgery símbolos (tokens) de forma predeterminada si el método del formulario no es GET.</span><span class="sxs-lookup"><span data-stu-id="961f9-174">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="961f9-175">La generación automática de símbolos (tokens) antiforgery para los elementos de formulario HTML se produce cuando el `<form>` etiqueta contiene el `method="post"` atributo y cualquiera de las siguientes son verdaderas:</span><span class="sxs-lookup"><span data-stu-id="961f9-175">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="961f9-176">El atributo de acción está vacío (`action=""`).</span><span class="sxs-lookup"><span data-stu-id="961f9-176">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="961f9-177">No se especifica el atributo de acción (`<form method="post">`).</span><span class="sxs-lookup"><span data-stu-id="961f9-177">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="961f9-178">Se puede deshabilitar la generación automática de símbolos (tokens) antiforgery para los elementos de formulario HTML:</span><span class="sxs-lookup"><span data-stu-id="961f9-178">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="961f9-179">Deshabilitar explícitamente antiforgery tokens con el `asp-antiforgery` atributo:</span><span class="sxs-lookup"><span data-stu-id="961f9-179">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="961f9-180">El elemento de formulario es darse de alta en horizontal de aplicaciones auxiliares de etiquetas mediante el uso de la aplicación auxiliar de etiqueta [! símbolos de desactivación](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="961f9-180">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="961f9-181">Quitar el `FormTagHelper` de la vista.</span><span class="sxs-lookup"><span data-stu-id="961f9-181">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="961f9-182">La `FormTagHelper` puede quitarse desde una vista agregando la siguiente directiva a la vista Razor:</span><span class="sxs-lookup"><span data-stu-id="961f9-182">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="961f9-183">[Las páginas de Razor](xref:mvc/razor-pages/index) están protegidos automáticamente frente a XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="961f9-183">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="961f9-184">Para obtener más información, consulte [XSRF/CSRF y páginas de Razor](xref:mvc/razor-pages/index#xsrf).</span><span class="sxs-lookup"><span data-stu-id="961f9-184">For more information, see [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf).</span></span>

<span data-ttu-id="961f9-185">El enfoque más común para defenderse contra ataques CSRF consiste en usar la *patrón del Token Sincronizador* (STP).</span><span class="sxs-lookup"><span data-stu-id="961f9-185">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="961f9-186">STP se utiliza cuando el usuario solicita una página con datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="961f9-186">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="961f9-187">El servidor envía un token asociado con la identidad del usuario actual al cliente.</span><span class="sxs-lookup"><span data-stu-id="961f9-187">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="961f9-188">El cliente devuelve el token en el servidor para la comprobación.</span><span class="sxs-lookup"><span data-stu-id="961f9-188">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="961f9-189">Si el servidor recibe un token que no coincide con la identidad del usuario autenticado, se rechaza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="961f9-189">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="961f9-190">El token es único y no previstos.</span><span class="sxs-lookup"><span data-stu-id="961f9-190">The token is unique and unpredictable.</span></span> <span data-ttu-id="961f9-191">El token puede usarse también para asegurarse de la secuencia correcta de una serie de solicitudes (por ejemplo, garantizar la secuencia de solicitud de: página 1 &ndash; página 2 &ndash; página 3).</span><span class="sxs-lookup"><span data-stu-id="961f9-191">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="961f9-192">Todos los formularios en plantillas de MVC de ASP.NET Core y páginas de Razor generan tokens antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-192">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="961f9-193">El siguiente par de ejemplos de vista genera tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="961f9-193">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="961f9-194">Agregar explícitamente un token antiforgery a una `<form>` elemento sin usar aplicaciones auxiliares de etiquetas con la aplicación auxiliar HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="961f9-194">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="961f9-195">En cada uno de los casos anteriores, ASP.NET Core agrega un campo de formulario oculto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="961f9-195">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="961f9-196">ASP.NET Core incluye tres [filtros](xref:mvc/controllers/filters) para trabajar con tokens antiforgery:</span><span class="sxs-lookup"><span data-stu-id="961f9-196">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="961f9-197">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="961f9-197">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="961f9-198">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="961f9-198">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="961f9-199">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="961f9-199">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="961f9-200">Opciones de antiforgery</span><span class="sxs-lookup"><span data-stu-id="961f9-200">Antiforgery options</span></span>

<span data-ttu-id="961f9-201">Personalizar [opciones antiforgery](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="961f9-201">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="961f9-202">Opción</span><span class="sxs-lookup"><span data-stu-id="961f9-202">Option</span></span> | <span data-ttu-id="961f9-203">Descripción</span><span class="sxs-lookup"><span data-stu-id="961f9-203">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="961f9-204">Cookie</span><span class="sxs-lookup"><span data-stu-id="961f9-204">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="961f9-205">Determina la configuración utilizada para crear la cookie antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-205">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="961f9-206">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="961f9-206">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="961f9-207">El dominio de la cookie.</span><span class="sxs-lookup"><span data-stu-id="961f9-207">The domain of the cookie.</span></span> <span data-ttu-id="961f9-208">Tiene como valor predeterminado `null`.</span><span class="sxs-lookup"><span data-stu-id="961f9-208">Defaults to `null`.</span></span> <span data-ttu-id="961f9-209">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="961f9-209">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="961f9-210">La alternativa recomendada es Cookie.Domain.</span><span class="sxs-lookup"><span data-stu-id="961f9-210">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="961f9-211">CookieName</span><span class="sxs-lookup"><span data-stu-id="961f9-211">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="961f9-212">El nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="961f9-212">The name of the cookie.</span></span> <span data-ttu-id="961f9-213">Si no está establecida, el sistema genera un nombre único que comienza con la [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="961f9-213">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="961f9-214">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="961f9-214">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="961f9-215">La alternativa recomendada es Cookie.Name.</span><span class="sxs-lookup"><span data-stu-id="961f9-215">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="961f9-216">CookiePath</span><span class="sxs-lookup"><span data-stu-id="961f9-216">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="961f9-217">La ruta de acceso establecido en la cookie.</span><span class="sxs-lookup"><span data-stu-id="961f9-217">The path set on the cookie.</span></span> <span data-ttu-id="961f9-218">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="961f9-218">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="961f9-219">La alternativa recomendada es Cookie.Path.</span><span class="sxs-lookup"><span data-stu-id="961f9-219">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="961f9-220">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="961f9-220">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="961f9-221">El nombre del campo oculto del formulario utilizado por el sistema antiforgery para representar antiforgery tokens en las vistas.</span><span class="sxs-lookup"><span data-stu-id="961f9-221">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="961f9-222">HeaderName</span><span class="sxs-lookup"><span data-stu-id="961f9-222">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="961f9-223">El nombre del encabezado utilizado por el sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-223">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="961f9-224">Si `null`, el sistema considera que solo los datos de formulario.</span><span class="sxs-lookup"><span data-stu-id="961f9-224">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="961f9-225">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="961f9-225">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="961f9-226">Especifica si se requiere SSL por el sistema antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-226">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="961f9-227">Si `true`, se producirá un error en las solicitudes sin SSL.</span><span class="sxs-lookup"><span data-stu-id="961f9-227">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="961f9-228">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="961f9-228">Defaults to `false`.</span></span> <span data-ttu-id="961f9-229">Esta propiedad está obsoleta y se quitará en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="961f9-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="961f9-230">La alternativa recomendada es establecer Cookie.SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="961f9-230">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="961f9-231">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="961f9-231">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="961f9-232">Especifica si se debe suprimir la generación de la `X-Frame-Options` encabezado.</span><span class="sxs-lookup"><span data-stu-id="961f9-232">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="961f9-233">De forma predeterminada, el encabezado se genera con un valor de "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="961f9-233">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="961f9-234">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="961f9-234">Defaults to `false`.</span></span> |

<span data-ttu-id="961f9-235">Para obtener más información, consulte [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span><span class="sxs-lookup"><span data-stu-id="961f9-235">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="961f9-236">Configurar características antiforgery con IAntiforgery</span><span class="sxs-lookup"><span data-stu-id="961f9-236">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="961f9-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) proporciona la API para configurar características antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-237">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="961f9-238">`IAntiforgery` se puede solicitar en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="961f9-238">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="961f9-239">En el ejemplo siguiente se usa el middleware de página principal de la aplicación para generar un token de antiforgery y enviarlo en la respuesta como una cookie (mediante la convención de nomenclatura de manera predeterminada Angular que se describe más adelante en este tema):</span><span class="sxs-lookup"><span data-stu-id="961f9-239">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="961f9-240">Requerir la validación antiforgery</span><span class="sxs-lookup"><span data-stu-id="961f9-240">Require antiforgery validation</span></span>

<span data-ttu-id="961f9-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) es un filtro de acción que se puede aplicar a una acción individual, un controlador, o de forma global.</span><span class="sxs-lookup"><span data-stu-id="961f9-241">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="961f9-242">Las solicitudes realizadas a las acciones que se ha aplicado este filtro se bloquean a menos que la solicitud incluye un token de antiforgery válido.</span><span class="sxs-lookup"><span data-stu-id="961f9-242">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="961f9-243">El `ValidateAntiForgeryToken` atributo requiere un token para las solicitudes a los métodos de acción que decora, incluidas las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="961f9-243">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="961f9-244">Si el `ValidateAntiForgeryToken` atributo se aplica en todos los controladores de la aplicación, puede reemplazarse por el `IgnoreAntiforgeryToken` atributo.</span><span class="sxs-lookup"><span data-stu-id="961f9-244">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="961f9-245">ASP.NET Core no admite la adición de tokens antiforgery a solicitudes GET automáticamente.</span><span class="sxs-lookup"><span data-stu-id="961f9-245">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="961f9-246">Validar automáticamente los tokens antiforgery para solo los métodos HTTP no seguros</span><span class="sxs-lookup"><span data-stu-id="961f9-246">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="961f9-247">Las aplicaciones ASP.NET Core no generan antiforgery tokens para los métodos HTTP seguros (GET, HEAD, opciones y seguimiento).</span><span class="sxs-lookup"><span data-stu-id="961f9-247">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="961f9-248">En lugar de aplicar ampliamente el `ValidateAntiForgeryToken` atributo y, a continuación, reemplazar con `IgnoreAntiforgeryToken` atributos, los [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) atributo se puede usar.</span><span class="sxs-lookup"><span data-stu-id="961f9-248">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="961f9-249">Este atributo funciona de forma idéntica a la `ValidateAntiForgeryToken` de atributo, salvo que no requiere tokens para las solicitudes realizadas con los siguientes métodos HTTP:</span><span class="sxs-lookup"><span data-stu-id="961f9-249">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="961f9-250">GET</span><span class="sxs-lookup"><span data-stu-id="961f9-250">GET</span></span>
* <span data-ttu-id="961f9-251">HEAD</span><span class="sxs-lookup"><span data-stu-id="961f9-251">HEAD</span></span>
* <span data-ttu-id="961f9-252">OPCIONES</span><span class="sxs-lookup"><span data-stu-id="961f9-252">OPTIONS</span></span>
* <span data-ttu-id="961f9-253">TRACE</span><span class="sxs-lookup"><span data-stu-id="961f9-253">TRACE</span></span>

<span data-ttu-id="961f9-254">Se recomienda usar `AutoValidateAntiforgeryToken` ampliamente para escenarios de API no.</span><span class="sxs-lookup"><span data-stu-id="961f9-254">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="961f9-255">Esto garantiza que las acciones de entrada están protegidas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="961f9-255">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="961f9-256">La alternativa es omitir antiforgery símbolos (tokens) de forma predeterminada, a menos que `ValidateAntiForgeryToken` se aplica a los métodos de acción individuales.</span><span class="sxs-lookup"><span data-stu-id="961f9-256">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="961f9-257">No esté más probable es que en este escenario para un método de acción POST que desee dejar protegida por error, salir de la aplicación sea vulnerable a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="961f9-257">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="961f9-258">Todas las publicaciones deben enviar el token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="961f9-258">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="961f9-259">Las API no tienen un mecanismo automático para el envío de la parte no cookie del token.</span><span class="sxs-lookup"><span data-stu-id="961f9-259">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="961f9-260">La implementación probablemente depende de la implementación del código de cliente.</span><span class="sxs-lookup"><span data-stu-id="961f9-260">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="961f9-261">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="961f9-261">Some examples are shown below:</span></span>

<span data-ttu-id="961f9-262">Ejemplo de nivel de clase:</span><span class="sxs-lookup"><span data-stu-id="961f9-262">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="961f9-263">Ejemplo global:</span><span class="sxs-lookup"><span data-stu-id="961f9-263">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="961f9-264">Reemplazo global o atributos antiforgery de controlador</span><span class="sxs-lookup"><span data-stu-id="961f9-264">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="961f9-265">El [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro se utiliza para eliminar la necesidad de un token antiforgery para una acción determinada (o controlador).</span><span class="sxs-lookup"><span data-stu-id="961f9-265">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="961f9-266">Cuando se aplica, invalida este filtro `ValidateAntiForgeryToken` y `AutoValidateAntiforgeryToken` filtros especificados en un nivel más alto (global o en un controlador).</span><span class="sxs-lookup"><span data-stu-id="961f9-266">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="961f9-267">Tokens de actualización después de la autenticación</span><span class="sxs-lookup"><span data-stu-id="961f9-267">Refresh tokens after authentication</span></span>

<span data-ttu-id="961f9-268">Símbolos (tokens) se debe actualizar después de que el usuario se autenticó mediante redirige al usuario a una vista o página de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="961f9-268">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="961f9-269">JavaScript, AJAX y SPAs</span><span class="sxs-lookup"><span data-stu-id="961f9-269">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="961f9-270">En las aplicaciones tradicionales basadas en HTML, antiforgery símbolos (tokens) se pasa al servidor mediante campos ocultos de formulario.</span><span class="sxs-lookup"><span data-stu-id="961f9-270">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="961f9-271">En las aplicaciones modernas basadas en JavaScript y SPAs, muchas de las solicitudes se realizan mediante programación.</span><span class="sxs-lookup"><span data-stu-id="961f9-271">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="961f9-272">Estas solicitudes de AJAX pueden usar otras técnicas (por ejemplo, los encabezados de solicitud o cookies) para enviar el token.</span><span class="sxs-lookup"><span data-stu-id="961f9-272">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="961f9-273">Si se usan cookies para almacenar los tokens de autenticación y para autenticar las solicitudes de API en el servidor, CSRF es un posible problema.</span><span class="sxs-lookup"><span data-stu-id="961f9-273">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="961f9-274">Si se usa almacenamiento local para almacenar el token, podría ser mitigada vulnerabilidad CSRF porque valores desde el almacenamiento local no se envían automáticamente al servidor con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="961f9-274">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="961f9-275">Por lo tanto, puede utilizar almacenamiento local para almacenar el token antiforgery en el cliente y enviar el token como un encabezado de solicitud es un enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="961f9-275">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="961f9-276">JavaScript</span><span class="sxs-lookup"><span data-stu-id="961f9-276">JavaScript</span></span>

<span data-ttu-id="961f9-277">Uso de JavaScript con vistas, el token de puede crearse con un servicio desde dentro de la vista.</span><span class="sxs-lookup"><span data-stu-id="961f9-277">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="961f9-278">Insertar el [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servicio en la vista y llame a [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="961f9-278">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="961f9-279">Este enfoque elimina la necesidad de tratar directamente con la configuración de cookies del servidor o para leerlos desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="961f9-279">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="961f9-280">El ejemplo anterior utiliza JavaScript para leer el valor del campo oculto para el encabezado de POST de AJAX.</span><span class="sxs-lookup"><span data-stu-id="961f9-280">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="961f9-281">JavaScript puede tokens de acceso en las cookies y usar el contenido de la cookie para crear un encabezado con el valor del token.</span><span class="sxs-lookup"><span data-stu-id="961f9-281">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="961f9-282">Suponiendo que la secuencia de comandos las solicitudes para enviar el token en un encabezado denominado `X-CSRF-TOKEN`, configurar el servicio antiforgery para buscar la `X-CSRF-TOKEN` encabezado:</span><span class="sxs-lookup"><span data-stu-id="961f9-282">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="961f9-283">En el ejemplo siguiente se utiliza JavaScript para realizar una solicitud de AJAX con el encabezado adecuado:</span><span class="sxs-lookup"><span data-stu-id="961f9-283">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="961f9-284">AngularJS</span><span class="sxs-lookup"><span data-stu-id="961f9-284">AngularJS</span></span>

<span data-ttu-id="961f9-285">AngularJS usa una convención para dirección CSRF.</span><span class="sxs-lookup"><span data-stu-id="961f9-285">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="961f9-286">Si el servidor envía una cookie con el nombre `XSRF-TOKEN`, AngularJS `$http` servicio agrega el valor de la cookie a un encabezado cuando envía una solicitud al servidor.</span><span class="sxs-lookup"><span data-stu-id="961f9-286">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="961f9-287">Este proceso es automático.</span><span class="sxs-lookup"><span data-stu-id="961f9-287">This process is automatic.</span></span> <span data-ttu-id="961f9-288">El encabezado no tiene que establecerse de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="961f9-288">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="961f9-289">El nombre de encabezado es `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="961f9-289">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="961f9-290">El servidor debe detectar este encabezado y validar su contenido.</span><span class="sxs-lookup"><span data-stu-id="961f9-290">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="961f9-291">Para la API de ASP.NET básica trabajar con esta convención:</span><span class="sxs-lookup"><span data-stu-id="961f9-291">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="961f9-292">Configurar la aplicación para proporcionar un token en una cookie denominada `XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="961f9-292">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="961f9-293">Configurar el servicio para que busque un encabezado denominado antiforgery `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="961f9-293">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="961f9-294">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="961f9-294">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="961f9-295">Extender antiforgery</span><span class="sxs-lookup"><span data-stu-id="961f9-295">Extend antiforgery</span></span>

<span data-ttu-id="961f9-296">El [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo permite a los desarrolladores extender el comportamiento del sistema anti-CSRF por datos adicionales de ida y vuelta de cada token.</span><span class="sxs-lookup"><span data-stu-id="961f9-296">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="961f9-297">El [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) método se llama cada vez que se genera un token de campo y el valor devuelto está incrustado en el token generado.</span><span class="sxs-lookup"><span data-stu-id="961f9-297">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="961f9-298">Un implementador podría devolver una marca de tiempo, un nonce o cualquier otro valor y, a continuación, llame a [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) para validar estos datos cuando se valida el token.</span><span class="sxs-lookup"><span data-stu-id="961f9-298">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="961f9-299">Nombre de usuario del cliente ya está incrustada en los tokens generados, por lo que no es necesario incluir esta información.</span><span class="sxs-lookup"><span data-stu-id="961f9-299">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="961f9-300">Si un token incluye datos suplementarios pero no `IAntiForgeryAdditionalDataProvider` está configurado, no validados los datos suplementarios.</span><span class="sxs-lookup"><span data-stu-id="961f9-300">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="961f9-301">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="961f9-301">Additional resources</span></span>

* <span data-ttu-id="961f9-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) en [Abrir proyecto de seguridad de aplicación Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="961f9-302">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
