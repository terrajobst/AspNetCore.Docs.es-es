---
title: Usar autenticación de cookies sin ASP.NET Core Identity
author: rick-anderson
description: Obtener una explicación del uso de autenticación de cookies sin ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: f3e02b357a83cf5fc4b9fcdc79b2fbe80da98507
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824756"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="0f2b0-103">Usar autenticación de cookies sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0f2b0-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="0f2b0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0f2b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0f2b0-105">Como ha visto en los temas de autenticación anteriores, [ASP.NET Core Identity](xref:security/authentication/identity) es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="0f2b0-106">Sin embargo, desea utilizar su propia lógica de autenticación personalizada con la autenticación basada en cookies a veces.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="0f2b0-107">Puede usar la autenticación basada en cookies como un proveedor de autenticación independiente sin ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="0f2b0-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0f2b0-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0f2b0-109">Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="0f2b0-110">Utilice el nombre de usuario de correo electrónico "maria.rodriguez@contoso.com" y cualquier contraseña para iniciar sesión el usuario.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="0f2b0-111">El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="0f2b0-112">En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="0f2b0-113">Para obtener información sobre la migración de la autenticación basada en cookies de ASP.NET Core 1.x a 2.0, consulte [migrar autenticación e Identity a ASP.NET Core 2.0 tema (autenticación basada en cookies)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="0f2b0-114">Para usar ASP.NET Core Identity, consulte el [Introducción a Identity](xref:security/authentication/identity) tema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="0f2b0-115">Configuración</span><span class="sxs-lookup"><span data-stu-id="0f2b0-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0f2b0-116">Si la aplicación no usa el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete (versión 2.1.0 o más adelante).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="0f2b0-117">En el `ConfigureServices` método, cree el servicio de Middleware de autenticación con el `AddAuthentication` y `AddCookie` métodos:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="0f2b0-118">`AuthenticationScheme` pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="0f2b0-119">`AuthenticationScheme` es útil cuando hay varias instancias de la autenticación con cookies y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="0f2b0-120">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="0f2b0-121">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="0f2b0-122">Esquema de autenticación de la aplicación es diferente de esquema de autenticación de cookies de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="0f2b0-123">Cuando no se proporciona un esquema de autenticación de cookies para <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, usa [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="0f2b0-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="0f2b0-124">La cookie de autenticación <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propiedad está establecida en `true` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-124">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="0f2b0-125">Cuando un visitante del sitio no ha dado su consentimiento para la recopilación de datos, se permiten las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-125">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="0f2b0-126">Para obtener más información, consulta <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-126">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="0f2b0-127">En el `Configure` método, use el `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-127">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="0f2b0-128">Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-128">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="0f2b0-129">**Opciones de AddCookie**</span><span class="sxs-lookup"><span data-stu-id="0f2b0-129">**AddCookie Options**</span></span>

<span data-ttu-id="0f2b0-130">El [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) clase se usa para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-130">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="0f2b0-131">Opción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-131">Option</span></span> | <span data-ttu-id="0f2b0-132">Descripción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-132">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0f2b0-133">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="0f2b0-133">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-134">Proporciona la ruta de acceso para proporcionar un 302 encontrado (redirección de URL) cuando se desencadena mediante `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-134">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="0f2b0-135">El valor predeterminado es `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-135">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="0f2b0-136">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="0f2b0-136">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-137">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creadas por el servicio de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-137">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="0f2b0-138">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="0f2b0-138">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-139">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-139">The domain name where the cookie is served.</span></span> <span data-ttu-id="0f2b0-140">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-140">By default, this is the host name of the request.</span></span> <span data-ttu-id="0f2b0-141">El explorador sólo envía la cookie en solicitudes a un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-141">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="0f2b0-142">Es posible que desee ajustar esta opción para que las cookies disponibles para cualquier host en el dominio.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-142">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="0f2b0-143">Por ejemplo, establecer el dominio de cookies en `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-143">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="0f2b0-144">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="0f2b0-144">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-145">Una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-145">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="0f2b0-146">Cambiar este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-146">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="0f2b0-147">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-147">The default value is `true`.</span></span> |
| [<span data-ttu-id="0f2b0-148">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="0f2b0-148">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-149">Establece el nombre de la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-149">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="0f2b0-150">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="0f2b0-150">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-151">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-151">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="0f2b0-152">Si tiene una aplicación que se ejecuta en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-152">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="0f2b0-153">Al hacerlo, solo está disponible en las solicitudes a la cookie `/app1` y cualquier aplicación debajo de ella.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-153">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="0f2b0-154">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="0f2b0-154">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-155">Indica si el explorador debe permitir la cookie que se adjuntará a solo las solicitudes del mismo sitio (`SameSiteMode.Strict`) o solicitudes entre sitios mediante los métodos HTTP seguros y las solicitudes del mismo sitio (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-155">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="0f2b0-156">Cuando se establece en `SameSiteMode.None`, no se establece el valor del encabezado de cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-156">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="0f2b0-157">Tenga en cuenta que [Middleware de cookies directiva](#cookie-policy-middleware) podría sobrescribir el valor que proporcione.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-157">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="0f2b0-158">Para admitir la autenticación de OAuth, el valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-158">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="0f2b0-159">Para obtener más información, consulte [autenticación OAuth interrumpida debido a la directiva de cookies de SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-159">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="0f2b0-160">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="0f2b0-160">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-161">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-161">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="0f2b0-162">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-162">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="0f2b0-163">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="0f2b0-163">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-164">Establece el `DataProtectionProvider` que se usa para crear el valor predeterminado `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-164">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="0f2b0-165">Si el `TicketDataFormat` propiedad está establecida, el `DataProtectionProvider` no se usa la opción.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-165">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="0f2b0-166">Si no se proporciona, se usa el proveedor de protección de datos de la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-166">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="0f2b0-167">Eventos</span><span class="sxs-lookup"><span data-stu-id="0f2b0-167">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-168">El controlador llama a métodos en el proveedor que proporcionan el control de aplicaciones en ciertos puntos de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-168">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="0f2b0-169">Si `Events` no siempre, se proporciona una instancia predeterminada que no hace nada cuando se llama a los métodos.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-169">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="0f2b0-170">EventsType</span><span class="sxs-lookup"><span data-stu-id="0f2b0-170">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-171">Utiliza como el tipo de servicio para obtener el `Events` instancia en lugar de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-171">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="0f2b0-172">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0f2b0-172">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-173">El `TimeSpan` tras el que expira el vale de autenticación que se almacena dentro de la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-173">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="0f2b0-174">`ExpireTimeSpan` se agrega a la hora actual para crear la hora de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-174">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="0f2b0-175">El `ExpiredTimeSpan` valor siempre se entra en el cifrado AuthTicket verificada por el servidor.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-175">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="0f2b0-176">También podría entrar en el [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) encabezado, pero solo si `IsPersistent` está establecido.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-176">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="0f2b0-177">Para establecer `IsPersistent` a `true`, configure el [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) pasa a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-177">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="0f2b0-178">El valor predeterminado de `ExpireTimeSpan` es 14 días.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-178">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="0f2b0-179">LoginPath</span><span class="sxs-lookup"><span data-stu-id="0f2b0-179">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-180">Proporciona la ruta de acceso para proporcionar un 302 encontrado (redirección de URL) cuando se desencadena mediante `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-180">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="0f2b0-181">La dirección URL actual que generó el 401 se agrega a la `LoginPath` como un parámetro de cadena de consulta denominado por la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-181">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="0f2b0-182">Una vez que una solicitud para el `LoginPath` una nueva identidad de inicio de sesión, se concede el `ReturnUrlParameter` valor se utiliza para redirigir el explorador a la dirección URL que ocasionó que el código de estado sin autorización original.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-182">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="0f2b0-183">El valor predeterminado es `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-183">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="0f2b0-184">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="0f2b0-184">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-185">Si el `LogoutPath` se proporciona al controlador, a continuación, redirige una solicitud a esa ruta de acceso en función del valor de la `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-185">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="0f2b0-186">El valor predeterminado es `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-186">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="0f2b0-187">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="0f2b0-187">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-188">Determina el nombre del parámetro de cadena de consulta que se anexa el controlador para una respuesta 302 encontrado (redirección de URL).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-188">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="0f2b0-189">`ReturnUrlParameter` se utiliza cuando llega una solicitud en el `LoginPath` o `LogoutPath` para devolver el explorador a la dirección URL original después de realiza la acción de inicio de sesión o cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-189">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="0f2b0-190">El valor predeterminado es `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-190">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="0f2b0-191">SessionStore</span><span class="sxs-lookup"><span data-stu-id="0f2b0-191">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-192">Contenedor opcional que se usa para almacenar la identidad a través de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-192">An optional container used to store identity across requests.</span></span> <span data-ttu-id="0f2b0-193">Cuando se utiliza, solo un identificador de sesión se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-193">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="0f2b0-194">`SessionStore` puede utilizarse para mitigar posibles problemas con identidades de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-194">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="0f2b0-195">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="0f2b0-195">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-196">Una marca que indica si se debe emitir una cookie nueva con un tiempo de expiración actualizada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-196">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="0f2b0-197">Esto puede ocurrir en cualquier solicitud donde el actual período de expiración de cookie más del 50% expiró.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-197">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="0f2b0-198">La nueva fecha de expiración se mueve hacia delante a la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-198">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="0f2b0-199">Un [tiempo de expiración absoluta cookie](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-199">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="0f2b0-200">Un tiempo de expiración absoluta puede mejorar la seguridad de la aplicación al limitar la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-200">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="0f2b0-201">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-201">The default value is `true`.</span></span> |
| [<span data-ttu-id="0f2b0-202">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="0f2b0-202">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-203">El `TicketDataFormat` se usa para proteger y desproteger la identidad y otras propiedades que se almacenan en el valor de cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-203">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="0f2b0-204">Si no se proporciona un `TicketDataFormat` se crea utilizando el [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-204">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="0f2b0-205">Validate</span><span class="sxs-lookup"><span data-stu-id="0f2b0-205">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="0f2b0-206">Método que comprueba que las opciones son válidas.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-206">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="0f2b0-207">Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-207">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0f2b0-208">ASP.NET Core 1.x usa cookies [middleware](xref:fundamentals/middleware/index) que serializa una entidad de seguridad del usuario en una cookie cifrada.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-208">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="0f2b0-209">En solicitudes posteriores, la cookie se valida y se vuelve a crear la entidad de seguridad y se asigna a la `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-209">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="0f2b0-210">Instalar el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete de NuGet en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-210">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="0f2b0-211">Este paquete contiene el middleware de cookies.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-211">This package contains the cookie middleware.</span></span>

<span data-ttu-id="0f2b0-212">Use la `UseCookieAuthentication` método en el `Configure` método en su *Startup.cs* archivo antes de `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-212">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="0f2b0-213">**Opciones de CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="0f2b0-213">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="0f2b0-214">El [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) clase se usa para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-214">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="0f2b0-215">Opción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-215">Option</span></span> | <span data-ttu-id="0f2b0-216">Descripción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-216">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0f2b0-217">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="0f2b0-217">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-218">Establece el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-218">Sets the authentication scheme.</span></span> <span data-ttu-id="0f2b0-219">`AuthenticationScheme` es útil cuando hay varias instancias de autenticación y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-219">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="0f2b0-220">Establecer el `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-220">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="0f2b0-221">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-221">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="0f2b0-222">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="0f2b0-222">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-223">Establece un valor para indicar que la autenticación con cookies debe ejecutar en cada solicitud y se intentan validar y reconstruir a cualquier entidad serializada que creó.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-223">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="0f2b0-224">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="0f2b0-224">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-225">Si es true, el middleware de autenticación controla desafíos automática.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-225">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="0f2b0-226">Si es false, el middleware de autenticación solo modifica las respuestas cuando lo indique explícitamente la `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-226">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="0f2b0-227">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="0f2b0-227">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-228">El emisor que se usará para la [emisor](/dotnet/api/system.security.claims.claim.issuer) propiedad en las notificaciones creados por el middleware de autenticación de cookies.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-228">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="0f2b0-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="0f2b0-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-230">El nombre de dominio donde se sirve la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-230">The domain name where the cookie is served.</span></span> <span data-ttu-id="0f2b0-231">De forma predeterminada, este es el nombre de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-231">By default, this is the host name of the request.</span></span> <span data-ttu-id="0f2b0-232">El explorador sólo sirve la cookie para un nombre de host coincidente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-232">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="0f2b0-233">Es posible que desee ajustar esta opción para que las cookies disponibles para cualquier host en el dominio.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-233">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="0f2b0-234">Por ejemplo, establecer el dominio de cookies en `.contoso.com` pone a disposición `contoso.com`, `www.contoso.com`, y `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-234">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="0f2b0-235">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="0f2b0-235">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-236">Una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-236">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="0f2b0-237">Cambiar este valor a `false` permite que los scripts del lado cliente para tener acceso a la cookie y se puede abrir la aplicación al robo de cookies debe tener la aplicación un [scripting entre sitios (XSS)](xref:security/cross-site-scripting) vulnerabilidad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-237">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="0f2b0-238">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-238">The default value is `true`.</span></span> |
| [<span data-ttu-id="0f2b0-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="0f2b0-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-240">Se utiliza para aislar las aplicaciones que se ejecutan en el mismo nombre de host.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-240">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="0f2b0-241">Si tiene una aplicación que se ejecuta en `/app1` y desea restringir las cookies a esa aplicación, establezca el `CookiePath` propiedad `/app1`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-241">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="0f2b0-242">Al hacerlo, solo está disponible en las solicitudes a la cookie `/app1` y cualquier aplicación debajo de ella.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-242">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="0f2b0-243">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="0f2b0-243">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-244">Una marca que indica si la cookie creada debe limitarse a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o el mismo protocolo que la solicitud (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-244">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="0f2b0-245">El valor predeterminado es `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-245">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="0f2b0-246">Descripción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-246">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-247">Información adicional sobre el tipo de autenticación que está disponible para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-247">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="0f2b0-248">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="0f2b0-248">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-249">El `TimeSpan` tras el que expira el vale de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-249">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="0f2b0-250">Se agrega a la hora actual para crear la hora de expiración para el vale.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-250">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="0f2b0-251">Para usar `ExpireTimeSpan`, debe establecer `IsPersistent` a `true` en el `AuthenticationProperties` pasa a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-251">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="0f2b0-252">El valor predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-252">The default value is 14 days.</span></span> |
| [<span data-ttu-id="0f2b0-253">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="0f2b0-253">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="0f2b0-254">Una marca que indica si la fecha de expiración de cookie restablece cuando haya más de la mitad de la `ExpireTimeSpan` ha transcurrido el intervalo.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-254">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="0f2b0-255">La nueva hora de expiración se mueve hacia delante a la fecha actual más el `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-255">The new expiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="0f2b0-256">Un [tiempo de expiración absoluta cookie](xref:security/authentication/cookie#absolute-cookie-expiration) puede establecerse mediante el `AuthenticationProperties` al llamar a la clase `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-256">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="0f2b0-257">Un tiempo de expiración absoluta puede mejorar la seguridad de la aplicación al limitar la cantidad de tiempo que la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-257">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="0f2b0-258">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-258">The default value is `true`.</span></span> |

<span data-ttu-id="0f2b0-259">Establecer `CookieAuthenticationOptions` para el Middleware de autenticación de cookies en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-259">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="0f2b0-260">Middleware de la directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-260">Cookie Policy Middleware</span></span>

<span data-ttu-id="0f2b0-261">[Middleware de cookies directiva](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) habilita capacidades de directiva de cookies en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-261">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="0f2b0-262">Agregar el middleware a la canalización de procesamiento de la aplicación es el orden de minúsculas; solo afecta a los componentes registrados después de él en la canalización.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-262">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="0f2b0-263">El [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) proporcionado para el Middleware de la directiva de cookies le permiten controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de la cookie cuando se agrega o se eliminan las cookies.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-263">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="0f2b0-264">Propiedad</span><span class="sxs-lookup"><span data-stu-id="0f2b0-264">Property</span></span> | <span data-ttu-id="0f2b0-265">Descripción</span><span class="sxs-lookup"><span data-stu-id="0f2b0-265">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="0f2b0-266">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="0f2b0-266">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="0f2b0-267">Afecta a si las cookies deben estar HttpOnly, que es una marca que indica si la cookie debe estar accesible sólo a los servidores.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-267">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="0f2b0-268">El valor predeterminado es `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-268">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="0f2b0-269">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="0f2b0-269">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="0f2b0-270">Afecta al atributo del mismo sitio de la cookie (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-270">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="0f2b0-271">El valor predeterminado es `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-271">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="0f2b0-272">Esta opción está disponible para ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-272">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="0f2b0-273">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="0f2b0-273">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="0f2b0-274">Se llama cuando se anexa una cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-274">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="0f2b0-275">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="0f2b0-275">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="0f2b0-276">Se llama cuando se elimina una cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-276">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="0f2b0-277">Secure</span><span class="sxs-lookup"><span data-stu-id="0f2b0-277">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="0f2b0-278">Determina si las cookies deben estar seguro.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-278">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="0f2b0-279">El valor predeterminado es `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-279">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="0f2b0-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)</span><span class="sxs-lookup"><span data-stu-id="0f2b0-280">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="0f2b0-281">El valor predeterminado `MinimumSameSitePolicy` es el valor `SameSiteMode.Lax` para permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-281">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="0f2b0-282">Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-282">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="0f2b0-283">Aunque esta configuración la interrumpe OAuth2 y otros esquemas de autenticación de origen cruzado, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no dependen de procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-283">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="0f2b0-284">La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-284">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="0f2b0-285">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="0f2b0-285">MinimumSameSitePolicy</span></span> | <span data-ttu-id="0f2b0-286">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="0f2b0-286">Cookie.SameSite</span></span> | <span data-ttu-id="0f2b0-287">Configuración de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="0f2b0-287">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="0f2b0-288">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="0f2b0-288">SameSiteMode.None</span></span>     | <span data-ttu-id="0f2b0-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="0f2b0-289">SameSiteMode.None</span></span><br><span data-ttu-id="0f2b0-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-291">SameSiteMode.Strict</span></span> | <span data-ttu-id="0f2b0-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="0f2b0-292">SameSiteMode.None</span></span><br><span data-ttu-id="0f2b0-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-293">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-294">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-294">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0f2b0-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-295">SameSiteMode.Lax</span></span>      | <span data-ttu-id="0f2b0-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="0f2b0-296">SameSiteMode.None</span></span><br><span data-ttu-id="0f2b0-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-298">SameSiteMode.Strict</span></span> | <span data-ttu-id="0f2b0-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-300">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-300">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-301">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-301">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0f2b0-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-302">SameSiteMode.Strict</span></span>   | <span data-ttu-id="0f2b0-303">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="0f2b0-303">SameSiteMode.None</span></span><br><span data-ttu-id="0f2b0-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="0f2b0-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="0f2b0-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-305">SameSiteMode.Strict</span></span> | <span data-ttu-id="0f2b0-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-306">SameSiteMode.Strict</span></span><br><span data-ttu-id="0f2b0-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-307">SameSiteMode.Strict</span></span><br><span data-ttu-id="0f2b0-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="0f2b0-308">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="0f2b0-309">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="0f2b0-309">Create an authentication cookie</span></span>

<span data-ttu-id="0f2b0-310">Para crear una cookie que contiene información de usuario, debe construir un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-310">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="0f2b0-311">La información de usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-311">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0f2b0-312">Crear un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con las [notificación](/dotnet/api/system.security.claims.claim)s y llamada [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-312">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0f2b0-313">Llame a [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="0f2b0-314">`SignInAsync` crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="0f2b0-315">Si no especifica un `AuthenticationScheme`, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="0f2b0-316">En segundo plano, el cifrado que usa es ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection) sistema.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system.</span></span> <span data-ttu-id="0f2b0-317">Si se va a hospedar la aplicación en varias máquinas, equilibrio de carga entre aplicaciones o usar una granja de servidores web, entonces debe [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo conjunto de claves y el identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="0f2b0-318">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="0f2b0-318">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0f2b0-319">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="0f2b0-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0f2b0-320">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="0f2b0-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="0f2b0-321">Si no usa `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que usó al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-321">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="0f2b0-322">En caso contrario, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-322">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="0f2b0-323">Reaccione ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="0f2b0-323">React to back-end changes</span></span>

<span data-ttu-id="0f2b0-324">Una vez que se crea una cookie, se convierte en el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-324">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="0f2b0-325">Incluso si deshabilita a un usuario en los sistemas de back-end, el sistema de autenticación de cookies no tiene ningún conocimiento de este, y un usuario permanece abierta siempre y cuando su cookie es válida.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-325">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="0f2b0-326">El [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) eventos en ASP.NET Core 2.x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) método en ASP.NET Core 1.x puede usarse para interceptar y reemplazar validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-326">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="0f2b0-327">Este enfoque reduce el riesgo de revocados a los usuarios acceden a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-327">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="0f2b0-328">Un enfoque de validación de la cookie se basa en realizar el seguimiento de cuándo se ha modificado la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-328">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="0f2b0-329">Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si la cookie sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-329">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="0f2b0-330">Para implementar este escenario, la base de datos, que se implementa en `IUserRepository` para este ejemplo, almacena una `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-330">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="0f2b0-331">Cuando se actualiza cualquier usuario en la base de datos, el `LastChanged` valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-331">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="0f2b0-332">Con el fin de invalidar una cookie cuando los cambios de la base de datos según la `LastChanged` valor, cree la cookie con un `LastChanged` de notificación que contiene el actual `LastChanged` valor de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-332">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0f2b0-333">Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="0f2b0-333">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="0f2b0-334">Un ejemplo es similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-334">An example looks like the following:</span></span>

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

<span data-ttu-id="0f2b0-335">Registrar la instancia de eventos durante el registro del servicio de cookie en el `ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-335">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="0f2b0-336">Proporcionar un registro de servicio con ámbito para su `CustomCookieAuthenticationEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-336">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0f2b0-337">Para implementar una invalidación para el `ValidateAsync` eventos, escribir un método con la firma siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-337">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="0f2b0-338">ASP.NET Core Identity implementa esta comprobación como parte de su [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="0f2b0-338">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="0f2b0-339">Un ejemplo es similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-339">An example looks like the following:</span></span>

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

<span data-ttu-id="0f2b0-340">Registrar el evento durante la configuración de autenticación de cookies en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="0f2b0-340">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

<span data-ttu-id="0f2b0-341">Considere una situación en la que se actualiza el nombre del usuario &mdash; una decisión que no afectan a la seguridad de cualquier manera.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-341">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="0f2b0-342">Si desea actualizar sin destruir datos de la entidad de seguridad de usuario, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-342">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="0f2b0-343">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-343">The approach described here is triggered on every request.</span></span> <span data-ttu-id="0f2b0-344">Esto puede dar lugar a una reducción del rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-344">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="0f2b0-345">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="0f2b0-345">Persistent cookies</span></span>

<span data-ttu-id="0f2b0-346">Desea que la cookie para conservar entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-346">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="0f2b0-347">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla de verificación "Recordar mi cuenta" en el inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-347">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="0f2b0-348">El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres del explorador.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-348">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="0f2b0-349">Se respeta cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-349">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="0f2b0-350">Si la cookie expira mientras se cierra el explorador, el explorador borrará la cookie de una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-350">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="0f2b0-351">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) clase reside en el `Microsoft.AspNetCore.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-351">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="0f2b0-352">El [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) clase reside en el `Microsoft.AspNetCore.Http.Authentication` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-352">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="0f2b0-353">Expiración de cookie absoluta</span><span class="sxs-lookup"><span data-stu-id="0f2b0-353">Absolute cookie expiration</span></span>

<span data-ttu-id="0f2b0-354">Puede establecer un tiempo de expiración absoluta con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-354">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="0f2b0-355">Para crear una cookie persistente, también debe establecer `IsPersistent`; en caso contrario, la cookie se crea con una duración basados en sesión y puede expirar antes o después de la autenticación de vale que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-355">To create a persistent cookie, you must also set `IsPersistent`; otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="0f2b0-356">Cuando `ExpiresUtc` se establece en `SignInAsync`, invalida el valor de la `ExpireTimeSpan` opción de `CookieAuthenticationOptions`, si establece.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-356">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="0f2b0-357">El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una duración de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-357">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="0f2b0-358">Esto omite cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="0f2b0-358">This ignores any sliding expiration settings previously configured.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0f2b0-359">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0f2b0-359">Additional resources</span></span>

* [<span data-ttu-id="0f2b0-360">Los cambios de autenticación 2.0 o anuncio de la migración</span><span class="sxs-lookup"><span data-stu-id="0f2b0-360">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="0f2b0-361">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="0f2b0-361">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
