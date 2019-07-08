---
title: Usar autenticación de cookies sin ASP.NET Core Identity
author: rick-anderson
description: Obtenga información sobre cómo usar la autenticación de cookies sin ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622742"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="c70a0-103">Usar autenticación de cookies sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="c70a0-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="c70a0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c70a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c70a0-105">ASP.NET Core Identity es un proveedor de autenticación completo y completa para crear y mantener los inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="c70a0-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="c70a0-106">Sin embargo, se puede usar un proveedor de autenticación de la autenticación basada en cookies sin ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="c70a0-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="c70a0-107">Para obtener más información, consulta <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="c70a0-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="c70a0-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c70a0-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c70a0-109">Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="c70a0-110">Use la **correo electrónico** nombre de usuario `maria.rodriguez@contoso.com` y cualquier contraseña para iniciar sesión el usuario.</span><span class="sxs-lookup"><span data-stu-id="c70a0-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="c70a0-111">El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="c70a0-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="c70a0-112">En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c70a0-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="c70a0-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="c70a0-113">Configuration</span></span>

<span data-ttu-id="c70a0-114">Si la aplicación no usa el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paquete.</span><span class="sxs-lookup"><span data-stu-id="c70a0-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="c70a0-115">En el `Startup.ConfigureServices` método, cree el servicio de Middleware de autenticación con el <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> y <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> métodos:</span><span class="sxs-lookup"><span data-stu-id="c70a0-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="c70a0-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> pasa al `AddAuthentication` establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="c70a0-117">`AuthenticationScheme` es útil cuando hay varias instancias de la autenticación con cookies y desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="c70a0-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="c70a0-118">Establecer el `AuthenticationScheme` a [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) proporciona un valor de "Cookies" para el esquema.</span><span class="sxs-lookup"><span data-stu-id="c70a0-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="c70a0-119">Puede proporcionar cualquier valor de cadena que distingue el esquema.</span><span class="sxs-lookup"><span data-stu-id="c70a0-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="c70a0-120">Esquema de autenticación de la aplicación es diferente de esquema de autenticación de cookies de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="c70a0-121">Cuando no se proporciona un esquema de autenticación de cookies para <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, usa `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="c70a0-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="c70a0-122">La cookie de autenticación <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> propiedad está establecida en `true` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c70a0-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="c70a0-123">Cuando un visitante del sitio no ha dado su consentimiento para la recopilación de datos, se permiten las cookies de autenticación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="c70a0-124">Para obtener más información, consulta <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="c70a0-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="c70a0-125">En el `Startup.Configure` método, llame a la `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="c70a0-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="c70a0-126">Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="c70a0-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="c70a0-127">La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> clase se usa para configurar las opciones de proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="c70a0-128">Establecer `CookieAuthenticationOptions` en la configuración del servicio para la autenticación en el `Startup.ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="c70a0-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="c70a0-129">Middleware de la directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="c70a0-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="c70a0-130">[Middleware de cookies directiva](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita capacidades de directiva de cookie.</span><span class="sxs-lookup"><span data-stu-id="c70a0-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="c70a0-131">Agregar el middleware a la canalización de procesamiento de la aplicación es el orden confidencial&mdash;solo afecta a los componentes de nivel inferior registrados en la canalización.</span><span class="sxs-lookup"><span data-stu-id="c70a0-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="c70a0-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado para el Middleware de la directiva de cookies para controlar características globales del procesamiento de la cookie y el enlace en los controladores de procesamiento de la cookie cuando se agrega o se eliminan las cookies.</span><span class="sxs-lookup"><span data-stu-id="c70a0-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="c70a0-133">El valor predeterminado <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> es el valor `SameSiteMode.Lax` para permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="c70a0-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="c70a0-134">Estrictamente aplicar una directiva del mismo sitio de `SameSiteMode.Strict`, establezca el `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="c70a0-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="c70a0-135">Aunque esta configuración la interrumpe OAuth2 y otros esquemas de autenticación de origen cruzado, eleva el nivel de seguridad de la cookie para otros tipos de aplicaciones que no dependen de procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="c70a0-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="c70a0-136">La configuración de directiva Middleware de cookies para `MinimumSameSitePolicy` pueden afectar a la configuración de `Cookie.SameSite` en `CookieAuthenticationOptions` valores según la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="c70a0-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="c70a0-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="c70a0-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="c70a0-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="c70a0-138">Cookie.SameSite</span></span> | <span data-ttu-id="c70a0-139">Configuración de Cookie.SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="c70a0-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="c70a0-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="c70a0-140">SameSiteMode.None</span></span>     | <span data-ttu-id="c70a0-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="c70a0-141">SameSiteMode.None</span></span><br><span data-ttu-id="c70a0-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="c70a0-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="c70a0-144">SameSiteMode.None</span></span><br><span data-ttu-id="c70a0-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="c70a0-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="c70a0-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="c70a0-148">SameSiteMode.None</span></span><br><span data-ttu-id="c70a0-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="c70a0-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="c70a0-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="c70a0-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="c70a0-155">SameSiteMode.None</span></span><br><span data-ttu-id="c70a0-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="c70a0-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="c70a0-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="c70a0-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="c70a0-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="c70a0-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="c70a0-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="c70a0-161">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="c70a0-161">Create an authentication cookie</span></span>

<span data-ttu-id="c70a0-162">Para crear una cookie que contiene información de usuario, construir un <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="c70a0-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="c70a0-163">La información de usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="c70a0-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="c70a0-164">Crear un <xref:System.Security.Claims.ClaimsIdentity> con las <xref:System.Security.Claims.Claim>s y llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> para iniciar sesión en el usuario:</span><span class="sxs-lookup"><span data-stu-id="c70a0-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="c70a0-165">`SignInAsync` crea una cookie cifrada y lo agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="c70a0-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="c70a0-166">Si `AuthenticationScheme` no se especifica, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c70a0-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="c70a0-167">ASP.NET Core [protección de datos](xref:security/data-protection/using-data-protection) system se utiliza para el cifrado.</span><span class="sxs-lookup"><span data-stu-id="c70a0-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="c70a0-168">Para una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones, o usar una granja de servidores web, [configurar la protección de datos](xref:security/data-protection/configuration/overview) para usar el mismo conjunto de claves y el identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="c70a0-169">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="c70a0-169">Sign out</span></span>

<span data-ttu-id="c70a0-170">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="c70a0-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="c70a0-171">Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookies") no se usa como el esquema (por ejemplo, "ContosoCookie"), proporcione el esquema usado al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="c70a0-172">En caso contrario, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c70a0-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="c70a0-173">Reaccione ante los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="c70a0-173">React to back-end changes</span></span>

<span data-ttu-id="c70a0-174">Una vez que se crea una cookie, la cookie es el único origen de identidad.</span><span class="sxs-lookup"><span data-stu-id="c70a0-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="c70a0-175">Si se deshabilita una cuenta de usuario en los sistemas back-end:</span><span class="sxs-lookup"><span data-stu-id="c70a0-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="c70a0-176">Sistema de autenticación de cookies de la aplicación continúa procesando las solicitudes basadas en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="c70a0-177">El usuario permanece conectado a la aplicación siempre y cuando la cookie de autenticación es válida.</span><span class="sxs-lookup"><span data-stu-id="c70a0-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="c70a0-178">El <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento puede utilizarse para interceptar y reemplazar validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="c70a0-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="c70a0-179">Validando la cookie en cada solicitud, reduce el riesgo de revocados a los usuarios acceden a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="c70a0-180">Un enfoque de validación de la cookie se basa en mantener un seguimiento de cuando cambia la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="c70a0-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="c70a0-181">Si la base de datos no se ha modificado desde que se emitió la cookie del usuario, no hay ninguna necesidad de volver a autenticar al usuario si la cookie sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="c70a0-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="c70a0-182">En la aplicación de ejemplo, la base de datos se implementa en `IUserRepository` y almacena una `LastChanged` valor.</span><span class="sxs-lookup"><span data-stu-id="c70a0-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="c70a0-183">Cuando un usuario se actualiza en la base de datos, el `LastChanged` valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="c70a0-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="c70a0-184">Con el fin de invalidar una cookie cuando los cambios de la base de datos según la `LastChanged` valor, cree la cookie con un `LastChanged` de notificación que contiene el actual `LastChanged` valor de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="c70a0-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="c70a0-185">Para implementar una invalidación para el `ValidatePrincipal` eventos, escribir un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="c70a0-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="c70a0-186">El siguiente es un ejemplo de implementación de `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="c70a0-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="c70a0-187">Registrar la instancia de eventos durante el registro del servicio de cookie en el `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="c70a0-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c70a0-188">Proporcione un [con ámbito de registro del servicio](xref:fundamentals/dependency-injection#service-lifetimes) para su `CustomCookieAuthenticationEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="c70a0-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="c70a0-189">Considere una situación en la que se actualiza el nombre del usuario&mdash;una decisión que no afectan a la seguridad de cualquier manera.</span><span class="sxs-lookup"><span data-stu-id="c70a0-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="c70a0-190">Si desea actualizar sin destruir datos de la entidad de seguridad de usuario, llame a `context.ReplacePrincipal` y establezca el `context.ShouldRenew` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="c70a0-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="c70a0-191">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="c70a0-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="c70a0-192">Validando las cookies de autenticación para todos los usuarios en cada solicitud puede producir una reducción del rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c70a0-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="c70a0-193">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="c70a0-193">Persistent cookies</span></span>

<span data-ttu-id="c70a0-194">Desea que la cookie para conservar entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="c70a0-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="c70a0-195">Esta persistencia solo debería habilitarse con el consentimiento del usuario explícita con una casilla de verificación "Recordar mi cuenta" en Inicio de sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="c70a0-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="c70a0-196">El fragmento de código siguiente crea una identidad y cookie correspondiente que sobrevive a través de los cierres del explorador.</span><span class="sxs-lookup"><span data-stu-id="c70a0-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="c70a0-197">Se respeta cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="c70a0-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="c70a0-198">Si la cookie expira mientras se cierra el explorador, el explorador borrará la cookie de una vez que se reinicie.</span><span class="sxs-lookup"><span data-stu-id="c70a0-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="c70a0-199">Establecer <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> a `true` en <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="c70a0-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="c70a0-200">Expiración de cookie absoluta</span><span class="sxs-lookup"><span data-stu-id="c70a0-200">Absolute cookie expiration</span></span>

<span data-ttu-id="c70a0-201">Se puede establecer un tiempo de expiración absoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="c70a0-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="c70a0-202">Para crear una cookie persistente, `IsPersistent` también debe establecerse.</span><span class="sxs-lookup"><span data-stu-id="c70a0-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="c70a0-203">En caso contrario, la cookie se crea con una duración basados en sesión y puede expirar antes o después el vale de autenticación que contiene.</span><span class="sxs-lookup"><span data-stu-id="c70a0-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="c70a0-204">Cuando `ExpiresUtc` está establecido, reemplaza el valor de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opción de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si establece.</span><span class="sxs-lookup"><span data-stu-id="c70a0-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="c70a0-205">El fragmento de código siguiente crea una identidad y cookie correspondiente que tiene una duración de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="c70a0-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="c70a0-206">Esto omite cualquier configuración de expiración deslizante configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="c70a0-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a><span data-ttu-id="c70a0-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c70a0-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="c70a0-208">Comprobaciones de la función basada en directivas</span><span class="sxs-lookup"><span data-stu-id="c70a0-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
