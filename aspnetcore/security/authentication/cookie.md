---
title: Usar autenticación de cookies sin ASP.NET Core Identity
author: rick-anderson
description: Obtener una explicación del uso de autenticación de cookies sin ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: ac1eec297d3efd1403990722f59c414ba4e5ddd9
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095806"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="67770-103">Usar autenticación de cookies sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="67770-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="67770-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="67770-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="67770-105">Como ha visto en los temas de autenticación anteriores, [ASP.NET Core Identity](xref:security/authentication/identity) es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="67770-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="67770-106">Sin embargo, desea utilizar su propia lógica de autenticación personalizada con la autenticación basada en cookies a veces.</span><span class="sxs-lookup"><span data-stu-id="67770-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="67770-107">Puede usar la autenticación basada en cookies como un proveedor de autenticación independiente sin ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="67770-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="67770-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67770-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="67770-109">Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="67770-110">Utilice el nombre de usuario de correo electrónico "maria.rodriguez@contoso.com" y cualquier contraseña para iniciar sesión el usuario.</span><span class="sxs-lookup"><span data-stu-id="67770-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="67770-111">El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="67770-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="67770-112">En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="67770-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="67770-113">Para obtener información sobre la migración de la autenticación basada en cookies de ASP.NET Core 1.x a 2.0, consulte [migrar autenticación e Identity a ASP.NET Core 2.0 tema (autenticación basada en cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="67770-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="67770-114">Para usar ASP.NET Core Identity, consulte el [Introducción a Identity](xref:security/authentication/identity) tema.</span><span class="sxs-lookup"><span data-stu-id="67770-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="67770-115">Configuración</span><span class="sxs-lookup"><span data-stu-id="67770-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="67770-117">Si la aplicación no usa el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete (versión 2.1.0 o más adelante).</span><span class="sxs-lookup"><span data-stu-id="67770-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="67770-118">En el `ConfigureServices` método, cree el servicio de Middleware de autenticación con el `AddAuthentication` y `AddCookie` métodos:</span><span class="sxs-lookup"><span data-stu-id="67770-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="67770-119">`AuthenticationScheme` pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="67770-120">`AuthenticationScheme` es útil cuando hay varias instancias de la autenticación con cookies y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="67770-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="67770-121">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="67770-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="67770-122">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="67770-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="67770-123">En el `Configure` método, use el `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="67770-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="67770-124">Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="67770-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="67770-125">**Opciones de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="67770-125">**AddCookie Options**</span></span>

<span data-ttu-id="67770-126">El [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) clase se usa para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="67770-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="67770-127">Opción</span><span class="sxs-lookup"><span data-stu-id="67770-127">Option</span></span> | <span data-ttu-id="67770-128">Descripción</span><span class="sxs-lookup"><span data-stu-id="67770-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="67770-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="67770-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="67770-130">Proporciona la ruta de acceso para proporcionar un 302 encontrado (redirección de URL) cuando se desencadena mediante `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="67770-131">El valor predeterminado es `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="67770-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="67770-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="67770-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="67770-133">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creadas por el servicio de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="67770-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="67770-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="67770-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="67770-135">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="67770-136">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="67770-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="67770-137">El explorador sólo envía la cookie en solicitudes a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="67770-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="67770-138">Es posible que desee ajustar esta opción para que las cookies disponibles para cualquier host en el dominio.</span><span class="sxs-lookup"><span data-stu-id="67770-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="67770-139">Por ejemplo, establecer el dominio de cookies en `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="67770-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="67770-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="67770-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="67770-141">Obtiene o establece la duración de una cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="67770-142">Actualmente, esta opción no funciona y quedarán obsoleta en ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="67770-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="67770-143">Use el `ExpireTimeSpan` opción para establecer la expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="67770-144">Para obtener más información, consulte [aclarar el comportamiento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="67770-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="67770-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="67770-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="67770-146">Una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="67770-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="67770-147">Cambiar este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="67770-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="67770-148">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="67770-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="67770-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="67770-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="67770-150">Establece el nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="67770-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="67770-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="67770-152">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="67770-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="67770-153">Si tiene una aplicación que se ejecuta en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="67770-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="67770-154">Al hacerlo, solo está disponible en las solicitudes a la cookie `/app1` y cualquier aplicación debajo de ella.</span><span class="sxs-lookup"><span data-stu-id="67770-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="67770-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="67770-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="67770-156">Indica si el explorador debe permitir la cookie que se adjuntará a solo las solicitudes del mismo sitio (`SameSiteMode.Strict`) o solicitudes entre sitios mediante los métodos HTTP seguros y las solicitudes del mismo sitio (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="67770-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="67770-157">Cuando se establece en `SameSiteMode.None`, no se establece el valor del encabezado de cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="67770-158">Tenga en cuenta que [Middleware de cookies directiva](#cookie-policy-middleware) podría sobrescribir el valor que proporcione.</span><span class="sxs-lookup"><span data-stu-id="67770-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="67770-159">Para admitir la autenticación de OAuth, el valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="67770-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="67770-160">Para obtener más información, consulte [autenticación OAuth interrumpida debido a la directiva de cookies de SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="67770-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="67770-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="67770-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="67770-162">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="67770-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="67770-163">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="67770-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="67770-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="67770-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="67770-165">Establece el `DataProtectionProvider` que se usa para crear el valor predeterminado `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="67770-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="67770-166">Si el `TicketDataFormat` propiedad está establecida, el `DataProtectionProvider` no se usa la opción.</span><span class="sxs-lookup"><span data-stu-id="67770-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="67770-167">Si no se proporciona, se usa el proveedor de protección de datos de la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="67770-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="67770-168">Eventos</span><span class="sxs-lookup"><span data-stu-id="67770-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="67770-169">El controlador llama a métodos en el proveedor que proporcionan el control de aplicaciones en ciertos puntos de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="67770-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="67770-170">Si `Events` no siempre, se proporciona una instancia predeterminada que no hace nada cuando se llama a los métodos.</span><span class="sxs-lookup"><span data-stu-id="67770-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="67770-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="67770-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="67770-172">Utiliza como el tipo de servicio para obtener el `Events` instancia en lugar de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="67770-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="67770-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="67770-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="67770-174">El `TimeSpan` tras el que expira el vale de autenticación que se almacena dentro de la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="67770-175">`ExpireTimeSpan` se agrega a la hora actual para crear la hora de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="67770-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="67770-176">El `ExpiredTimeSpan` valor siempre se entra en el cifrado AuthTicket verificada por el servidor.</span><span class="sxs-lookup"><span data-stu-id="67770-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="67770-177">También podría entrar en el [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) encabezado, pero solo si `IsPersistent` está establecido.</span><span class="sxs-lookup"><span data-stu-id="67770-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="67770-178">Para establecer `IsPersistent` a `true`, configure el [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) pasa a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="67770-179">El valor predeterminado de `ExpireTimeSpan` es 14 días.</span><span class="sxs-lookup"><span data-stu-id="67770-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="67770-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="67770-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="67770-181">Proporciona la ruta de acceso para proporcionar un 302 encontrado (redirección de URL) cuando se desencadena mediante `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="67770-182">La dirección URL actual que generó el 401 se agrega a la `LoginPath` como un parámetro de cadena de consulta denominado por la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="67770-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="67770-183">Una vez que una solicitud para el `LoginPath` una nueva identidad de inicio de sesión, se concede el `ReturnUrlParameter` valor se utiliza para redirigir el explorador a la dirección URL que ocasionó que el código de estado sin autorización original.</span><span class="sxs-lookup"><span data-stu-id="67770-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="67770-184">El valor predeterminado es `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="67770-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="67770-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="67770-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="67770-186">Si el `LogoutPath` se proporciona al controlador, a continuación, redirige una solicitud a esa ruta de acceso en función del valor de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="67770-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="67770-187">El valor predeterminado es `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="67770-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="67770-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="67770-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="67770-189">Determina el nombre del parámetro de cadena de consulta que se anexa el controlador para una respuesta 302 encontrado (redirección de URL).</span><span class="sxs-lookup"><span data-stu-id="67770-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="67770-190">`ReturnUrlParameter` se utiliza cuando llega una solicitud en el `LoginPath` o `LogoutPath` para devolver el explorador a la dirección URL original después de realiza la acción de inicio de sesión o cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="67770-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="67770-191">El valor predeterminado es `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="67770-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="67770-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="67770-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="67770-193">Contenedor opcional que se usa para almacenar la identidad a través de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="67770-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="67770-194">Cuando se utiliza, solo un identificador de sesión se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="67770-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="67770-195">`SessionStore` puede utilizarse para mitigar posibles problemas con identidades de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="67770-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="67770-196">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="67770-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="67770-197">Una marca que indica si se debe emitir una cookie nueva con un tiempo de expiración actualizada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="67770-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="67770-198">Esto puede ocurrir en cualquier solicitud donde el actual período de expiración de cookie más del 50% expiró.</span><span class="sxs-lookup"><span data-stu-id="67770-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="67770-199">La nueva fecha de expiración se mueve hacia delante a la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="67770-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="67770-200">Un [tiempo de expiración absoluta cookie](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="67770-201">Un tiempo de expiración absoluta puede mejorar la seguridad de la aplicación al limitar la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="67770-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="67770-202">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="67770-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="67770-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="67770-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="67770-204">El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="67770-205">Si no se proporciona un `TicketDataFormat` se crea utilizando el [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="67770-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="67770-206">Validar</span><span class="sxs-lookup"><span data-stu-id="67770-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="67770-207">Método que comprueba que las opciones son válidas.</span><span class="sxs-lookup"><span data-stu-id="67770-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="67770-208">Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="67770-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="67770-210">ASP.NET Core 1.x usa cookies [middleware](xref:fundamentals/middleware/index) que serializa una entidad de seguridad del usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="67770-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="67770-211">En solicitudes posteriores, la cookie se valida y se vuelve a crear la entidad de seguridad y se asigna a la `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="67770-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="67770-212">Instalar el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="67770-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="67770-213">Este paquete contiene el middleware de cookies.</span><span class="sxs-lookup"><span data-stu-id="67770-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="67770-214">Use la `UseCookieAuthentication` método en el `Configure` método en su *Startup.cs* archivo antes de `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="67770-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="67770-215">**Opciones de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="67770-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="67770-216">El [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) clase se usa para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="67770-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="67770-217">Opción</span><span class="sxs-lookup"><span data-stu-id="67770-217">Option</span></span> | <span data-ttu-id="67770-218">Descripción</span><span class="sxs-lookup"><span data-stu-id="67770-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="67770-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="67770-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="67770-220">Establece el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="67770-220">Sets the authentication scheme.</span></span> <span data-ttu-id="67770-221">`AuthenticationScheme` es útil cuando hay varias instancias de autenticación y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="67770-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="67770-222">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="67770-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="67770-223">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="67770-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="67770-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="67770-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="67770-225">Establece un valor para indicar que la autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir a cualquier entidad serializada que creó.</span><span class="sxs-lookup"><span data-stu-id="67770-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="67770-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="67770-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="67770-227">Si es true, el middleware de autenticación controla desafíos automática.</span><span class="sxs-lookup"><span data-stu-id="67770-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="67770-228">Si es false, el middleware de autenticación solo modifica las respuestas cuando lo indique explícitamente la `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="67770-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="67770-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="67770-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="67770-230">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creados por el middleware de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="67770-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="67770-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="67770-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="67770-232">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="67770-233">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="67770-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="67770-234">El explorador sólo sirve la cookie para un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="67770-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="67770-235">Es posible que desee ajustar esta opción para que las cookies disponibles para cualquier host en el dominio.</span><span class="sxs-lookup"><span data-stu-id="67770-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="67770-236">Por ejemplo, establecer el dominio de cookies en `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="67770-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="67770-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="67770-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="67770-238">Una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="67770-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="67770-239">Cambiar este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="67770-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="67770-240">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="67770-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="67770-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="67770-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="67770-242">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="67770-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="67770-243">Si tiene una aplicación que se ejecuta en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="67770-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="67770-244">Al hacerlo, solo está disponible en las solicitudes a la cookie `/app1` y cualquier aplicación debajo de ella.</span><span class="sxs-lookup"><span data-stu-id="67770-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="67770-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="67770-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="67770-246">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="67770-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="67770-247">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="67770-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="67770-248">Descripción</span><span class="sxs-lookup"><span data-stu-id="67770-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="67770-249">Información adicional sobre el tipo de autenticación que está disponible para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="67770-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="67770-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="67770-251">El `TimeSpan` tras el que expira el vale de autenticación.</span><span class="sxs-lookup"><span data-stu-id="67770-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="67770-252">Se agrega a la hora actual para crear la hora de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="67770-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="67770-253">Para usar `ExpireTimeSpan`, debe establecer `IsPersistent` a `true` en el `AuthenticationProperties` pasa a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="67770-254">El valor predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="67770-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="67770-255">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="67770-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="67770-256">Una marca que indica si la fecha de expiración de cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="67770-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="67770-257">La nueva hora exipiration se mueve hacia delante a la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="67770-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="67770-258">Un [tiempo de expiración absoluta cookie](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="67770-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="67770-259">Un tiempo de expiración absoluta puede mejorar la seguridad de la aplicación al limitar la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="67770-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="67770-260">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="67770-260">The default value is `true`.</span></span> |

<span data-ttu-id="67770-261">Establecer `CookieAuthenticationOptions` para el Middleware de autenticación de cookies en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="67770-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="67770-262">Middleware de la directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="67770-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="67770-263">[Middleware de cookies directiva](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita capacidades de directiva de cookies en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="67770-264">Agregar el middleware a la canalización de procesamiento de la aplicación es el orden de minúsculas; solo afecta a los componentes registrados después de él en la canalización.</span><span class="sxs-lookup"><span data-stu-id="67770-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="67770-265">El [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) proporcionado para el Middleware de la directiva de cookies le permiten controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de la cookie cuando se agrega o se eliminan las cookies.</span><span class="sxs-lookup"><span data-stu-id="67770-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="67770-266">Property</span><span class="sxs-lookup"><span data-stu-id="67770-266">Property</span></span> | <span data-ttu-id="67770-267">Descripción</span><span class="sxs-lookup"><span data-stu-id="67770-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="67770-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="67770-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="67770-269">Afecta a si las cookies deben estar HttpOnly, que es una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="67770-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="67770-270">El valor predeterminado es `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="67770-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="67770-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="67770-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="67770-272">Afecta al atributo del mismo sitio de la cookie (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="67770-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="67770-273">El valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="67770-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="67770-274">Esta opción está disponible para ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="67770-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="67770-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="67770-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="67770-276">Se llama cuando se anexa una cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="67770-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="67770-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="67770-278">Se llama cuando se elimina una cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="67770-279">Proteger</span><span class="sxs-lookup"><span data-stu-id="67770-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="67770-280">Determina si las cookies deben estar seguro.</span><span class="sxs-lookup"><span data-stu-id="67770-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="67770-281">El valor predeterminado es `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="67770-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="67770-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)</span><span class="sxs-lookup"><span data-stu-id="67770-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="67770-283">El valor predeterminado `MinimumSameSitePolicy` es el valor `SameSiteMode.Lax` para permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="67770-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="67770-284">Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="67770-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="67770-285">Aunque esta configuración la interrumpe OAuth2 y otros esquemas de autenticación de origen cruzado, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no dependen de procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="67770-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="67770-286">La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="67770-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="67770-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="67770-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="67770-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="67770-288">Cookie.SameSite</span></span> | <span data-ttu-id="67770-289">Configuración de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="67770-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="67770-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="67770-290">SameSiteMode.None</span></span>     | <span data-ttu-id="67770-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="67770-291">SameSiteMode.None</span></span><br><span data-ttu-id="67770-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="67770-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="67770-294">SameSiteMode.None</span></span><br><span data-ttu-id="67770-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="67770-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="67770-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="67770-298">SameSiteMode.None</span></span><br><span data-ttu-id="67770-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="67770-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="67770-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="67770-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="67770-305">SameSiteMode.None</span></span><br><span data-ttu-id="67770-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="67770-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="67770-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="67770-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="67770-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="67770-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="67770-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="67770-311">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="67770-311">Create an authentication cookie</span></span>

<span data-ttu-id="67770-312">Para crear una cookie que contiene información de usuario, debe construir un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="67770-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="67770-313">La información de usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="67770-315">Crear un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con las [notificación](/dotnet/api/system.security.claims.claim)s y llamada [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="67770-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="67770-317">Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="67770-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="67770-318">`SignInAsync` crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="67770-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="67770-319">Si no especifica un `AuthenticationScheme`, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="67770-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="67770-320">En segundo plano, el cifrado que usa es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="67770-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="67770-321">Si se va a hospedar la aplicación en varias máquinas, equilibrio de carga entre aplicaciones o usar una granja de servidores web, entonces debe [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo conjunto de claves y el identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="67770-322">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="67770-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="67770-324">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="67770-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="67770-326">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="67770-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="67770-327">Si no usa `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que usó al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="67770-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="67770-328">En caso contrario, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="67770-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="67770-329">Reaccione ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="67770-329">React to back-end changes</span></span>

<span data-ttu-id="67770-330">Una vez que se crea una cookie, se convierte en el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="67770-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="67770-331">Incluso si deshabilita a un usuario en los sistemas de back-end, el sistema de autenticación de cookies no tiene ningún conocimiento de este, y un usuario permanece abierta siempre y cuando su cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="67770-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="67770-332">El [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) eventos en ASP.NET Core 2.x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método en ASP.NET Core 1.x puede usarse para interceptar y reemplazar validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="67770-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="67770-333">Este enfoque reduce el riesgo de revocados a los usuarios acceden a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="67770-334">Un enfoque de validación de la cookie se basa en realizar el seguimiento de cuándo se ha modificado la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="67770-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="67770-335">Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si la cookie sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="67770-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="67770-336">Para implementar este escenario, la base de datos, que se implementa en `IUserRepository` para este ejemplo, almacena una `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="67770-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="67770-337">Cuando se actualiza cualquier usuario en la base de datos, el `LastChanged` valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="67770-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="67770-338">Con el fin de invalidar una cookie cuando los cambios de la base de datos según la `LastChanged` valor, cree la cookie con un `LastChanged` de notificación que contiene el actual `LastChanged` valor de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="67770-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="67770-340">Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="67770-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="67770-341">Un ejemplo es similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="67770-341">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="67770-342">Registrar la instancia de eventos durante el registro del servicio de cookie en el `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="67770-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="67770-343">Proporcionar un registro de servicio con ámbito para su `CustomCookieAuthenticationEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="67770-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="67770-345">Para implementar una invalidación para el `ValidateAsync` eventos, escribir un método con la firma siguiente:</span><span class="sxs-lookup"><span data-stu-id="67770-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="67770-346">ASP.NET Core Identity implementa esta comprobación como parte de su [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="67770-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="67770-347">Un ejemplo es similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="67770-347">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="67770-348">Registrar el evento durante la configuración de autenticación de cookies en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="67770-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="67770-349">Considere una situación en la que se actualiza el nombre del usuario &mdash; una decisión que no afectan a la seguridad de cualquier manera.</span><span class="sxs-lookup"><span data-stu-id="67770-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="67770-350">Si desea actualizar sin destruir datos de la entidad de seguridad de usuario, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="67770-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="67770-351">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="67770-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="67770-352">Esto puede dar lugar a una reducción del rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="67770-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="67770-353">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="67770-353">Persistent cookies</span></span>

<span data-ttu-id="67770-354">Desea que la cookie para conservar entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="67770-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="67770-355">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla "Recordar mi cuenta" en el inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="67770-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="67770-356">El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres del explorador.</span><span class="sxs-lookup"><span data-stu-id="67770-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="67770-357">Se respeta cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="67770-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="67770-358">Si la cookie expira mientras se cierra el explorador, el explorador borrará la cookie de una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="67770-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="67770-360">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) clase reside en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="67770-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="67770-362">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) clase reside en el `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="67770-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="67770-363">Expiración de cookie absoluta</span><span class="sxs-lookup"><span data-stu-id="67770-363">Absolute cookie expiration</span></span>

<span data-ttu-id="67770-364">Puede establecer un tiempo de expiración absoluta con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="67770-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="67770-365">También debe establecer `IsPersistent`; en caso contrario, `ExpiresUtc` se omite y se crea una cookie de sesión único.</span><span class="sxs-lookup"><span data-stu-id="67770-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="67770-366">Cuando `ExpiresUtc` se establece en `SignInAsync`, invalida el valor de la `ExpireTimeSpan` opción de `CookieAuthenticationOptions`, si establece.</span><span class="sxs-lookup"><span data-stu-id="67770-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="67770-367">El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una duración de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="67770-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="67770-368">Esto omite cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="67770-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="67770-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="67770-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="67770-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="67770-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="additional-resources"></a><span data-ttu-id="67770-371">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="67770-371">Additional resources</span></span>

* [<span data-ttu-id="67770-372">Los cambios de autenticación 2.0 o anuncio de la migración</span><span class="sxs-lookup"><span data-stu-id="67770-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="67770-373">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="67770-373">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
