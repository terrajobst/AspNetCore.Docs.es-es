---
title: Usar autenticación con cookies sin ASP.NET Core Identity
author: rick-anderson
description: Obtener una explicación del uso de autenticación con cookies sin ASP.NET Core Identity
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: bdaa0e3a5ce54d3822615ac57e22f4fd6beacdcb
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="69a84-103">Usar autenticación con cookies sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="69a84-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="69a84-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="69a84-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="69a84-105">Como ha visto en los temas de autenticación anteriores, [ASP.NET Core Identity](xref:security/authentication/identity) es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="69a84-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="69a84-106">Sin embargo, puede que desee utilizar su propia lógica de autenticación personalizada con la autenticación basada en cookies a veces.</span><span class="sxs-lookup"><span data-stu-id="69a84-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="69a84-107">Puede usar la autenticación basada en cookies como un proveedor de autenticación independiente sin ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="69a84-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="69a84-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="69a84-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="69a84-109">Para obtener información sobre migración autenticación basada en cookies de ASP.NET Core 1.x a 2.0, consulte [migrar autenticación e identidad al tema principal de ASP.NET 2.0 (autenticación basada en cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="69a84-109">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="69a84-110">Configuración</span><span class="sxs-lookup"><span data-stu-id="69a84-110">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="69a84-112">Si no está usando la [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), instale la versión 2.0 de la [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="69a84-112">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="69a84-113">En el `ConfigureServices` (método), crear el servicio de Middleware de autenticación con el `AddAuthentication` y `AddCookie` métodos:</span><span class="sxs-lookup"><span data-stu-id="69a84-113">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="69a84-114">`AuthenticationScheme` pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-114">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="69a84-115">`AuthenticationScheme` es útil cuando hay varias instancias de autenticación con cookies y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="69a84-115">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="69a84-116">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="69a84-116">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="69a84-117">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="69a84-117">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="69a84-118">En el `Configure` método, use la `UseAuthentication` método que se invoca el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="69a84-118">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="69a84-119">Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="69a84-119">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="69a84-120">**Opciones de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="69a84-120">**AddCookie Options**</span></span>

<span data-ttu-id="69a84-121">El [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) clase se utiliza para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="69a84-121">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="69a84-122">Opción</span><span class="sxs-lookup"><span data-stu-id="69a84-122">Option</span></span> | <span data-ttu-id="69a84-123">Descripción</span><span class="sxs-lookup"><span data-stu-id="69a84-123">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="69a84-124">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="69a84-124">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="69a84-125">Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-125">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="69a84-126">El valor predeterminado es `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="69a84-126">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="69a84-127">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="69a84-127">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="69a84-128">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creados por el servicio de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-128">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="69a84-129">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="69a84-129">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="69a84-130">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-130">The domain name where the cookie is served.</span></span> <span data-ttu-id="69a84-131">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="69a84-131">By default, this is the host name of the request.</span></span> <span data-ttu-id="69a84-132">El explorador sólo envía la cookie en las solicitudes a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="69a84-132">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="69a84-133">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="69a84-133">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="69a84-134">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="69a84-134">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="69a84-135">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="69a84-135">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="69a84-136">Obtiene o establece el tiempo de vida de una cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-136">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="69a84-137">Actualmente, esta opción no ops y quedarán obsoleta en ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="69a84-137">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="69a84-138">Use la `ExpireTimeSpan` opción para establecer la expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-138">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="69a84-139">Para obtener más información, consulte [aclarar el comportamiento de CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="69a84-139">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="69a84-140">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="69a84-140">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="69a84-141">Una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="69a84-141">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="69a84-142">Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="69a84-142">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="69a84-143">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="69a84-143">The default value is `true`.</span></span> |
| [<span data-ttu-id="69a84-144">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="69a84-144">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="69a84-145">Establece el nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-145">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="69a84-146">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="69a84-146">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="69a84-147">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="69a84-147">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="69a84-148">Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="69a84-148">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="69a84-149">Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="69a84-149">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="69a84-150">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="69a84-150">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="69a84-151">Indica si el explorador debe permitir la cookie que se adjuntará a sólo las solicitudes del mismo sitio (`SameSiteMode.Strict`) o solicitudes entre sitios mediante métodos de prueba de errores HTTP y solicitudes del mismo sitio (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="69a84-151">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="69a84-152">Cuando se establece en `SameSiteMode.None`, no se establece el valor del encabezado de cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-152">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="69a84-153">Tenga en cuenta que [Middleware de cookies directiva](#cookie-policy-middleware) podría sobrescribir el valor que se proporcione.</span><span class="sxs-lookup"><span data-stu-id="69a84-153">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="69a84-154">Para admitir la autenticación de OAuth, el valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="69a84-154">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="69a84-155">Para obtener más información, consulte [roto debido a la directiva de SameSite cookie de autenticación de OAuth](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="69a84-155">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="69a84-156">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="69a84-156">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="69a84-157">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="69a84-157">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="69a84-158">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="69a84-158">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="69a84-159">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="69a84-159">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="69a84-160">Establece el `DataProtectionProvider` que se utiliza para crear el valor predeterminado `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="69a84-160">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="69a84-161">Si el `TicketDataFormat` propiedad está establecida, el `DataProtectionProvider` opción no se utiliza.</span><span class="sxs-lookup"><span data-stu-id="69a84-161">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="69a84-162">Si no se proporciona, se utiliza el proveedor de protección de datos de la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="69a84-162">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="69a84-163">Eventos</span><span class="sxs-lookup"><span data-stu-id="69a84-163">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="69a84-164">El controlador llama a métodos en el proveedor que proporcionan el control de la aplicación en determinados puntos de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="69a84-164">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="69a84-165">Si `Events` no siempre, se proporciona una instancia predeterminada que no hace nada cuando se llaman a los métodos.</span><span class="sxs-lookup"><span data-stu-id="69a84-165">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="69a84-166">EventsType</span><span class="sxs-lookup"><span data-stu-id="69a84-166">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="69a84-167">Utiliza como el tipo de servicio para obtener el `Events` instancia en lugar de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="69a84-167">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="69a84-168">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="69a84-168">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="69a84-169">El `TimeSpan` tras el cual expira el vale de autenticación que se almacena dentro de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-169">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="69a84-170">`ExpireTimeSpan` se agrega a la hora actual para crear la fecha de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="69a84-170">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="69a84-171">El `ExpiredTimeSpan` siempre que el valor sea en el cifrado AuthTicket verificada por el servidor.</span><span class="sxs-lookup"><span data-stu-id="69a84-171">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="69a84-172">También puede ir a la [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) encabezado, pero solo si `IsPersistent` se establece.</span><span class="sxs-lookup"><span data-stu-id="69a84-172">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="69a84-173">Para establecer `IsPersistent` a `true`, configurar la [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) pasado a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-173">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="69a84-174">El valor predeterminado de `ExpireTimeSpan` es 14 días.</span><span class="sxs-lookup"><span data-stu-id="69a84-174">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="69a84-175">LoginPath</span><span class="sxs-lookup"><span data-stu-id="69a84-175">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="69a84-176">Proporciona la ruta de acceso para suministrar con un 302 encontrado (redireccionamiento de la dirección URL) cuando se desencadena por `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-176">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="69a84-177">La dirección URL actual que generó el 401 se agrega a la `LoginPath` como un parámetro de cadena de consulta denominado por la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="69a84-177">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="69a84-178">Una vez una solicitud para la `LoginPath` concede una nueva identidad de inicio de sesión, la `ReturnUrlParameter` valor se utiliza para redirigir el explorador a la dirección URL que produjo el código de estado sin autorización original.</span><span class="sxs-lookup"><span data-stu-id="69a84-178">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="69a84-179">El valor predeterminado es `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="69a84-179">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="69a84-180">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="69a84-180">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="69a84-181">Si el `LogoutPath` se proporciona al controlador, a continuación, redirige una solicitud a dicha ruta de acceso en función del valor de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="69a84-181">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="69a84-182">El valor predeterminado es `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="69a84-182">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="69a84-183">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="69a84-183">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="69a84-184">Determina el nombre del parámetro de cadena de consulta que se anexa el controlador para una respuesta 302 de Found (redireccionamiento de la dirección URL).</span><span class="sxs-lookup"><span data-stu-id="69a84-184">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="69a84-185">`ReturnUrlParameter` se utiliza cuando llega una solicitud en el `LoginPath` o `LogoutPath` para devolver el explorador a la dirección URL original después de realiza la acción de inicio de sesión o cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="69a84-185">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="69a84-186">El valor predeterminado es `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="69a84-186">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="69a84-187">SessionStore</span><span class="sxs-lookup"><span data-stu-id="69a84-187">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="69a84-188">Contenedor opcional que se utiliza para almacenar la identidad todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="69a84-188">An optional container used to store identity across requests.</span></span> <span data-ttu-id="69a84-189">Cuando se utiliza, solo un identificador de sesión se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="69a84-189">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="69a84-190">`SessionStore` puede utilizarse para mitigar los posibles problemas con identidades grandes.</span><span class="sxs-lookup"><span data-stu-id="69a84-190">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="69a84-191">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="69a84-191">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="69a84-192">Una marca que indica si se debe emitir una nueva cookie con una fecha de caducidad actualizada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="69a84-192">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="69a84-193">Esto puede suceder en cualquier solicitud que el período de expiración de cookie actual es más del 50% expira.</span><span class="sxs-lookup"><span data-stu-id="69a84-193">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="69a84-194">La nueva fecha de expiración se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="69a84-194">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="69a84-195">Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-195">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="69a84-196">Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="69a84-196">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="69a84-197">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="69a84-197">The default value is `true`.</span></span> |
| [<span data-ttu-id="69a84-198">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="69a84-198">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="69a84-199">El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-199">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="69a84-200">Si no se proporciona un `TicketDataFormat` se crea utilizando el [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="69a84-200">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="69a84-201">Validar</span><span class="sxs-lookup"><span data-stu-id="69a84-201">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="69a84-202">Método que compruebe que las opciones son válidas.</span><span class="sxs-lookup"><span data-stu-id="69a84-202">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="69a84-203">Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="69a84-203">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="69a84-205">ASP.NET Core 1.x usa cookies [middleware](xref:fundamentals/middleware/index) que serializa una entidad de seguridad de usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="69a84-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="69a84-206">En solicitudes posteriores, la cookie se valida y se vuelve a crear la entidad de seguridad y se asigna a la `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="69a84-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="69a84-207">Instalar el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="69a84-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="69a84-208">Este paquete contiene el middleware de cookies.</span><span class="sxs-lookup"><span data-stu-id="69a84-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="69a84-209">Use la `UseCookieAuthentication` método en el `Configure` método en su *Startup.cs* archivo antes de `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="69a84-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="69a84-210">**Opciones de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="69a84-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="69a84-211">El [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) clase se utiliza para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="69a84-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="69a84-212">Opción</span><span class="sxs-lookup"><span data-stu-id="69a84-212">Option</span></span> | <span data-ttu-id="69a84-213">Descripción</span><span class="sxs-lookup"><span data-stu-id="69a84-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="69a84-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="69a84-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="69a84-215">Establece el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="69a84-215">Sets the authentication scheme.</span></span> <span data-ttu-id="69a84-216">`AuthenticationScheme` es útil cuando hay varias instancias de autenticación y desea [autorizarse con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="69a84-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="69a84-217">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="69a84-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="69a84-218">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="69a84-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="69a84-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="69a84-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="69a84-220">Establece un valor para indicar que la autenticación con cookies debe ejecutarse en cada solicitud y que intentan validar y reconstruir cualquier entidad de seguridad serializado que creó.</span><span class="sxs-lookup"><span data-stu-id="69a84-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="69a84-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="69a84-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="69a84-222">Si es true, el middleware de autenticación controla desafíos automática.</span><span class="sxs-lookup"><span data-stu-id="69a84-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="69a84-223">Si false, el middleware de autenticación sólo altera las respuestas cuando indique explícitamente la `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="69a84-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="69a84-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="69a84-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="69a84-225">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en cualquier notificación creado por el middleware de autenticación de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="69a84-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="69a84-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="69a84-227">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="69a84-228">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="69a84-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="69a84-229">El explorador sólo sirve la cookie a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="69a84-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="69a84-230">Puede que desee ajustar esta opción para que las cookies disponibles para todos los hosts en el dominio.</span><span class="sxs-lookup"><span data-stu-id="69a84-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="69a84-231">Por ejemplo, establecer el dominio de cookies `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="69a84-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="69a84-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="69a84-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="69a84-233">Una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="69a84-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="69a84-234">Si cambia este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) una vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="69a84-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="69a84-235">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="69a84-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="69a84-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="69a84-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="69a84-237">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="69a84-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="69a84-238">Si tiene aplicaciones que se ejecutan en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="69a84-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="69a84-239">Al hacerlo, la cookie solo está disponible en las solicitudes a `/app1` y cualquier aplicación aparecen debajo de él.</span><span class="sxs-lookup"><span data-stu-id="69a84-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="69a84-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="69a84-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="69a84-241">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="69a84-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="69a84-242">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="69a84-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="69a84-243">Descripción</span><span class="sxs-lookup"><span data-stu-id="69a84-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="69a84-244">Información adicional sobre el tipo de autenticación que debe ponerse a disposición de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="69a84-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="69a84-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="69a84-246">El `TimeSpan` tras el cual expira el vale de autenticación.</span><span class="sxs-lookup"><span data-stu-id="69a84-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="69a84-247">Se agrega a la hora actual para crear la fecha de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="69a84-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="69a84-248">Usar `ExpireTimeSpan`, debe establecer `IsPersistent` a `true` en el `AuthenticationProperties` pasado a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="69a84-249">El valor predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="69a84-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="69a84-250">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="69a84-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="69a84-251">Una marca que indica si la fecha de expiración de la cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="69a84-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="69a84-252">La nueva hora de exipiration se mueve hacia delante como la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="69a84-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="69a84-253">Un [tiempo de expiración de cookie absoluta](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="69a84-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="69a84-254">Una hora de expiración absoluta puede mejorar la seguridad de la aplicación mediante la limitación de la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="69a84-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="69a84-255">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="69a84-255">The default value is `true`.</span></span> |

<span data-ttu-id="69a84-256">Establecer `CookieAuthenticationOptions` para el Middleware de autenticación de Cookie en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="69a84-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="69a84-257">Middleware de cookies directiva.</span><span class="sxs-lookup"><span data-stu-id="69a84-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="69a84-258">[Middleware de cookies directiva](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita las capacidades de directiva de cookie de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="69a84-259">Agregar el middleware a la canalización de procesamiento de la aplicación es el orden de minúsculas; solo afecta a los componentes registrados después de él en la canalización.</span><span class="sxs-lookup"><span data-stu-id="69a84-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="69a84-260">El [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) proporcionado para el Middleware de directiva de Cookie le permiten controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de cookie cuando se agrega o se eliminan las cookies.</span><span class="sxs-lookup"><span data-stu-id="69a84-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="69a84-261">Property</span><span class="sxs-lookup"><span data-stu-id="69a84-261">Property</span></span> | <span data-ttu-id="69a84-262">Descripción</span><span class="sxs-lookup"><span data-stu-id="69a84-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="69a84-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="69a84-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="69a84-264">Afecta a si las cookies deben estar HttpOnly, que es una marca que indica si la cookie debe ser accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="69a84-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="69a84-265">El valor predeterminado es `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="69a84-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="69a84-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="69a84-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="69a84-267">Afecta al atributo del mismo sitio de la cookie (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="69a84-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="69a84-268">El valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="69a84-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="69a84-269">Esta opción está disponible para el núcleo de ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="69a84-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="69a84-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="69a84-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="69a84-271">Se llama cuando se anexa una cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="69a84-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="69a84-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="69a84-273">Se llama cuando se elimina una cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="69a84-274">Proteger</span><span class="sxs-lookup"><span data-stu-id="69a84-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="69a84-275">Determina si las cookies deben estar seguro.</span><span class="sxs-lookup"><span data-stu-id="69a84-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="69a84-276">El valor predeterminado es `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="69a84-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="69a84-277">**MinimumSameSitePolicy** (ASP.NET 2.0 + sólo principal)</span><span class="sxs-lookup"><span data-stu-id="69a84-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="69a84-278">El valor predeterminado `MinimumSameSitePolicy` valor es `SameSiteMode.Lax` para permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="69a84-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="69a84-279">Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="69a84-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="69a84-280">Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no confían en el procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="69a84-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="69a84-281">La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="69a84-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="69a84-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="69a84-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="69a84-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="69a84-283">Cookie.SameSite</span></span> | <span data-ttu-id="69a84-284">Configuración de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="69a84-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="69a84-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="69a84-285">SameSiteMode.None</span></span>     | <span data-ttu-id="69a84-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="69a84-286">SameSiteMode.None</span></span><br><span data-ttu-id="69a84-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="69a84-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="69a84-289">SameSiteMode.None</span></span><br><span data-ttu-id="69a84-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="69a84-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="69a84-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="69a84-293">SameSiteMode.None</span></span><br><span data-ttu-id="69a84-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="69a84-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="69a84-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="69a84-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="69a84-300">SameSiteMode.None</span></span><br><span data-ttu-id="69a84-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="69a84-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="69a84-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="69a84-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="69a84-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="69a84-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="69a84-305">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="69a84-306">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="69a84-306">Creating an authentication cookie</span></span>

<span data-ttu-id="69a84-307">Para crear una cookie que contiene información de usuario, debe construir un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="69a84-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="69a84-308">La información de usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-308">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="69a84-310">Crear un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con cualquier necesario [notificación](/dotnet/api/system.security.claims.claim)s y llame al método [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="69a84-310">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="69a84-312">Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="69a84-312">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="69a84-313">`SignInAsync` crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="69a84-313">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="69a84-314">Si no se especifica un `AuthenticationScheme`, se utiliza el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="69a84-314">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="69a84-315">Tras los bastidores, el cifrado usado es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="69a84-315">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="69a84-316">Si aloja la aplicación en varios equipos, equilibrio de carga entre las aplicaciones o usar una granja de servidores web, debe [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo anillo de clave y el identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-316">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="69a84-317">Cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="69a84-317">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-318">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-318">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="69a84-319">Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="69a84-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="69a84-321">Para cerrar la sesión del usuario actual y eliminar las cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="69a84-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="69a84-322">Si no está usando `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que usó al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="69a84-322">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="69a84-323">En caso contrario, se utiliza el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="69a84-323">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="69a84-324">Reaccionar ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="69a84-324">Reacting to back-end changes</span></span>

<span data-ttu-id="69a84-325">Una vez que se crea una cookie, se convierte en el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="69a84-325">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="69a84-326">Incluso si se deshabilita a un usuario en los sistemas back-end, el sistema de autenticación de cookie no tiene ningún conocimiento de este, y un usuario permanece ha iniciado sesión como su cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="69a84-326">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="69a84-327">El [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) eventos en ASP.NET Core 2.x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método en ASP.NET Core 1.x puede usarse para interceptar y reemplazar la validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="69a84-327">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="69a84-328">Este enfoque reduce el riesgo de revocados a los usuarios obtener acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-328">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="69a84-329">Un enfoque de validación de la cookie se basa en realizar el seguimiento de cuándo se ha modificado la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="69a84-329">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="69a84-330">Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si su cookie sigue siendo válido.</span><span class="sxs-lookup"><span data-stu-id="69a84-330">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="69a84-331">Para implementar este escenario, la base de datos, que se implementa en `IUserRepository` en este ejemplo, almacena una `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="69a84-331">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="69a84-332">Cuando cualquier usuario se actualiza en la base de datos, la `LastChanged` valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="69a84-332">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="69a84-333">Con el fin de invalidar una cookie cuando los cambios de la base de datos se basan en el `LastChanged` valor, cree la cookie con un `LastChanged` notificación que contiene el actual `LastChanged` valor de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="69a84-333">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69a84-335">Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="69a84-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="69a84-336">Un ejemplo es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="69a84-336">An example looks like the following:</span></span>

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

<span data-ttu-id="69a84-337">Registrar la instancia de eventos durante el registro de servicio de cookie en el `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="69a84-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="69a84-338">Proporcionar un registro de servicio con ámbito para su `CustomCookieAuthenticationEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="69a84-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69a84-340">Para implementar una invalidación para el `ValidateAsync` eventos, escribir un método con la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="69a84-340">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="69a84-341">ASP.NET Core Identity implementa esta comprobación como parte de su [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="69a84-341">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="69a84-342">Un ejemplo es similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="69a84-342">An example looks like the following:</span></span>

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

<span data-ttu-id="69a84-343">Registrar el evento durante la configuración de autenticación de cookie en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="69a84-343">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="69a84-344">Considere la posibilidad de una situación en la que se actualiza el nombre del usuario &mdash; una decisión que no afectan a la seguridad de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="69a84-344">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="69a84-345">Si desea actualizar la entidad de seguridad de usuario de manera no destructiva, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="69a84-345">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="69a84-346">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="69a84-346">The approach described here is triggered on every request.</span></span> <span data-ttu-id="69a84-347">Esto puede dar lugar a una reducción del rendimiento de gran tamaño de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69a84-347">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="69a84-348">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="69a84-348">Persistent cookies</span></span>

<span data-ttu-id="69a84-349">Puede que desee la cookie que se conservan entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="69a84-349">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="69a84-350">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="69a84-350">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="69a84-351">El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres de explorador.</span><span class="sxs-lookup"><span data-stu-id="69a84-351">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="69a84-352">Se respetan los parámetros de expiración deslizante configurados previamente.</span><span class="sxs-lookup"><span data-stu-id="69a84-352">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="69a84-353">Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie de una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="69a84-353">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-354">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-354">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="69a84-355">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) clase reside en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="69a84-355">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-356">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-356">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="69a84-357">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) clase reside en el `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="69a84-357">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="69a84-358">Expiración de cookie absoluta</span><span class="sxs-lookup"><span data-stu-id="69a84-358">Absolute cookie expiration</span></span>

<span data-ttu-id="69a84-359">Puede establecer un tiempo de expiración absoluta con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="69a84-359">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="69a84-360">También debe establecer `IsPersistent`; en caso contrario, `ExpiresUtc` se omite y se crea una cookie de sesión único.</span><span class="sxs-lookup"><span data-stu-id="69a84-360">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="69a84-361">Cuando `ExpiresUtc` está establecido en `SignInAsync`, invalida el valor de la `ExpireTimeSpan` opción de `CookieAuthenticationOptions`, si se establece.</span><span class="sxs-lookup"><span data-stu-id="69a84-361">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="69a84-362">El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una validez de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="69a84-362">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="69a84-363">Esto omite cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="69a84-363">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69a84-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69a84-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69a84-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69a84-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="see-also"></a><span data-ttu-id="69a84-366">Vea también</span><span class="sxs-lookup"><span data-stu-id="69a84-366">See also</span></span>

* [<span data-ttu-id="69a84-367">Cambios de autenticación 2.0 / anuncio de migración</span><span class="sxs-lookup"><span data-stu-id="69a84-367">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="69a84-368">Limitación de la identidad por esquema</span><span class="sxs-lookup"><span data-stu-id="69a84-368">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="69a84-369">Autorización basada en notificaciones</span><span class="sxs-lookup"><span data-stu-id="69a84-369">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="69a84-370">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="69a84-370">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
