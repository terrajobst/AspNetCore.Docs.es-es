---
title: Usar la autenticación de cookies sin ASP.NET Core identidad
author: rick-anderson
description: Aprenda a usar la autenticación de cookies sin ASP.NET Core identidad.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
uid: security/authentication/cookie
ms.openlocfilehash: 76c7fc20c8870668ca7c65d975e2ed59f40f7dc8
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384826"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="7fa19-103">Usar la autenticación de cookies sin ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="7fa19-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="7fa19-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7fa19-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7fa19-105">ASP.NET Core Identity es un completo proveedor de autenticación completo para crear y mantener inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="7fa19-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="7fa19-106">Sin embargo, se puede usar un proveedor de autenticación de autenticación basada en cookies sin ASP.NET Core identidad.</span><span class="sxs-lookup"><span data-stu-id="7fa19-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="7fa19-107">Para obtener más información, consulta <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="7fa19-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="7fa19-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7fa19-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7fa19-109">Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotético, María Rodriguez, está codificada en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7fa19-110">Use la dirección `maria.rodriguez@contoso.com` de **correo electrónico** y cualquier contraseña para iniciar sesión en el usuario.</span><span class="sxs-lookup"><span data-stu-id="7fa19-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="7fa19-111">El usuario se autentica en el `AuthenticateUser` método del archivo *pages/Account/login. cshtml. CS* .</span><span class="sxs-lookup"><span data-stu-id="7fa19-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7fa19-112">En un ejemplo real, el usuario se autenticaría en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="7fa19-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="7fa19-113">Configuration</span></span>

<span data-ttu-id="7fa19-114">Si la aplicación no usa el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el paquete [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="7fa19-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="7fa19-115">En el `Startup.ConfigureServices` método, cree los servicios de middleware de autenticación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> con <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> los métodos y:</span><span class="sxs-lookup"><span data-stu-id="7fa19-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7fa19-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>se pasa `AddAuthentication` a establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="7fa19-117">`AuthenticationScheme`resulta útil cuando hay varias instancias de autenticación de cookies y se desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="7fa19-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7fa19-118">Si se establece en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) , se proporciona el valor "cookies" para el esquema. `AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="7fa19-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7fa19-119">Puede proporcionar cualquier valor de cadena que distinga el esquema.</span><span class="sxs-lookup"><span data-stu-id="7fa19-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="7fa19-120">El esquema de autenticación de la aplicación es diferente del esquema de autenticación de cookies de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="7fa19-121">Cuando no se proporciona un esquema de autenticación <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>de cookies `CookieAuthenticationDefaults.AuthenticationScheme` a, USA ("cookies").</span><span class="sxs-lookup"><span data-stu-id="7fa19-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="7fa19-122">La propiedad de la <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> cookie de autenticación está `true` establecida de forma predeterminada en.</span><span class="sxs-lookup"><span data-stu-id="7fa19-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="7fa19-123">Las cookies de autenticación se permiten cuando un visitante del sitio no ha dado su consentimiento a la recopilación de datos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="7fa19-124">Para obtener más información, consulta <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="7fa19-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="7fa19-125">En `Startup.Configure`, llame `UseAuthentication` a `UseAuthorization` y para establecer `HttpContext.User` la propiedad y ejecutar el middleware de autorización para las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="7fa19-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="7fa19-126">Llame a `UseAuthentication` los `UseAuthorization` métodos y antes `UseEndpoints`de llamar a:</span><span class="sxs-lookup"><span data-stu-id="7fa19-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="7fa19-127">La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> clase se utiliza para configurar las opciones del proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="7fa19-128">Establezca `CookieAuthenticationOptions` en la configuración del servicio para la autenticación `Startup.ConfigureServices` en el método:</span><span class="sxs-lookup"><span data-stu-id="7fa19-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="7fa19-129">Middleware de directiva de cookies</span><span class="sxs-lookup"><span data-stu-id="7fa19-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="7fa19-130">El [middleware de directiva de cookies](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita las funcionalidades de directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="7fa19-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="7fa19-131">Agregar el middleware a la canalización de procesamiento de la&mdash;aplicación es dependiente del orden. solo afecta a los componentes de nivel inferior registrados en la canalización.</span><span class="sxs-lookup"><span data-stu-id="7fa19-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="7fa19-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado al middleware de la Directiva de cookies para controlar las características globales del procesamiento de cookies y enlazar los controladores de procesamiento de cookies cuando se anexan o eliminan cookies.</span><span class="sxs-lookup"><span data-stu-id="7fa19-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="7fa19-133">El valor <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predeterminado es `SameSiteMode.Lax` permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="7fa19-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="7fa19-134">Para aplicar estrictamente una directiva del mismo sitio de `SameSiteMode.Strict`, `MinimumSameSitePolicy`establezca.</span><span class="sxs-lookup"><span data-stu-id="7fa19-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="7fa19-135">Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de las cookies para otros tipos de aplicaciones que no se basan en el procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="7fa19-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="7fa19-136">La configuración de middleware de la `MinimumSameSitePolicy` Directiva de cookies para `Cookie.SameSite` puede `CookieAuthenticationOptions` afectar a la configuración de en la configuración de acuerdo con la matriz siguiente.</span><span class="sxs-lookup"><span data-stu-id="7fa19-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="7fa19-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7fa19-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="7fa19-138">Cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="7fa19-138">Cookie.SameSite</span></span> | <span data-ttu-id="7fa19-139">Valor de cookie. SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="7fa19-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="7fa19-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-140">SameSiteMode.None</span></span>     | <span data-ttu-id="7fa19-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-141">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-144">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7fa19-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="7fa19-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-148">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7fa19-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="7fa19-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-155">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="7fa19-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="7fa19-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="7fa19-161">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="7fa19-161">Create an authentication cookie</span></span>

<span data-ttu-id="7fa19-162">Para crear una cookie que contiene información de usuario, <xref:System.Security.Claims.ClaimsPrincipal>construya un.</span><span class="sxs-lookup"><span data-stu-id="7fa19-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="7fa19-163">La información del usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="7fa19-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="7fa19-164">Cree un <xref:System.Security.Claims.ClaimsIdentity> con los s <xref:System.Security.Claims.Claim>necesarios y llame <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> al usuario para iniciar sesión:</span><span class="sxs-lookup"><span data-stu-id="7fa19-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="7fa19-165">`SignInAsync`crea una cookie cifrada y la agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="7fa19-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="7fa19-166">Si `AuthenticationScheme` no se especifica, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="7fa19-167">El sistema de [protección de datos](xref:security/data-protection/using-data-protection) de ASP.net Core se usa para el cifrado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="7fa19-168">En el caso de una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones o uso de una granja de servidores Web, [Configure la protección de datos](xref:security/data-protection/configuration/overview) para que use el mismo anillo de claves e identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="7fa19-169">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="7fa19-169">Sign out</span></span>

<span data-ttu-id="7fa19-170">Para cerrar la sesión del usuario actual y eliminar su cookie, <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>llame a:</span><span class="sxs-lookup"><span data-stu-id="7fa19-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="7fa19-171">Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookies") no se usa como esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que se usa al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="7fa19-172">De lo contrario, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="7fa19-173">Reaccionar a los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="7fa19-173">React to back-end changes</span></span>

<span data-ttu-id="7fa19-174">Una vez creada una cookie, ésta es la única fuente de identidad.</span><span class="sxs-lookup"><span data-stu-id="7fa19-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="7fa19-175">Si una cuenta de usuario está deshabilitada en los sistemas de servidor:</span><span class="sxs-lookup"><span data-stu-id="7fa19-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="7fa19-176">El sistema de autenticación de cookies de la aplicación continúa procesando solicitudes basadas en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="7fa19-177">El usuario permanecerá conectado en la aplicación siempre y cuando la cookie de autenticación sea válida.</span><span class="sxs-lookup"><span data-stu-id="7fa19-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="7fa19-178">El <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento se puede utilizar para interceptar e invalidar la validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="7fa19-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="7fa19-179">La validación de la cookie en cada solicitud mitiga el riesgo de que los usuarios revocados accedan a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="7fa19-180">Un enfoque para la validación de cookies se basa en realizar un seguimiento de cuándo cambia la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="7fa19-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="7fa19-181">Si no se ha cambiado la base de datos desde que se emitió la cookie del usuario, no es necesario volver a autenticar al usuario si la cookie sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="7fa19-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="7fa19-182">En la aplicación de ejemplo, la base de datos `IUserRepository` se implementa en `LastChanged` y almacena un valor.</span><span class="sxs-lookup"><span data-stu-id="7fa19-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="7fa19-183">Cuando se actualiza un usuario en la base de datos `LastChanged` , el valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="7fa19-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="7fa19-184">Para invalidar una cookie cuando la base de datos cambie según el `LastChanged` valor, cree la cookie con una `LastChanged` solicitud que contenga el `LastChanged` valor actual de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="7fa19-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="7fa19-185">Para implementar una invalidación para `ValidatePrincipal` el evento, escriba un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="7fa19-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="7fa19-186">El siguiente es un ejemplo de implementación `CookieAuthenticationEvents`de:</span><span class="sxs-lookup"><span data-stu-id="7fa19-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="7fa19-187">Registre la instancia de eventos durante el registro del servicio `Startup.ConfigureServices` de cookies en el método.</span><span class="sxs-lookup"><span data-stu-id="7fa19-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7fa19-188">Proporcione un [registro de servicio de ámbito](xref:fundamentals/dependency-injection#service-lifetimes) para `CustomCookieAuthenticationEvents` la clase:</span><span class="sxs-lookup"><span data-stu-id="7fa19-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="7fa19-189">Considere una situación en la que el nombre del usuario se&mdash;actualice una decisión que no afecte a la seguridad de ningún modo.</span><span class="sxs-lookup"><span data-stu-id="7fa19-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="7fa19-190">Si desea actualizar de la entidad de seguridad de usuario de una no destructiva `context.ReplacePrincipal` , llame a `context.ShouldRenew` y establezca `true`la propiedad en.</span><span class="sxs-lookup"><span data-stu-id="7fa19-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="7fa19-191">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="7fa19-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="7fa19-192">La validación de las cookies de autenticación para todos los usuarios de cada solicitud puede producir una gran penalización del rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="7fa19-193">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="7fa19-193">Persistent cookies</span></span>

<span data-ttu-id="7fa19-194">Puede que desee que la cookie se conserve entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="7fa19-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="7fa19-195">Esta persistencia solo se debe habilitar con el consentimiento explícito del usuario con una casilla "Recordarme" al iniciar sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="7fa19-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="7fa19-196">El siguiente fragmento de código crea una identidad y la cookie correspondiente que sobrevive a través de los cierres del explorador.</span><span class="sxs-lookup"><span data-stu-id="7fa19-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="7fa19-197">Se respetan los valores de expiración que se hayan configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="7fa19-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="7fa19-198">Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie una vez que se reinicia.</span><span class="sxs-lookup"><span data-stu-id="7fa19-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="7fa19-199">Se <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>establece enen:`true`</span><span class="sxs-lookup"><span data-stu-id="7fa19-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="7fa19-200">Expiración absoluta de cookies</span><span class="sxs-lookup"><span data-stu-id="7fa19-200">Absolute cookie expiration</span></span>

<span data-ttu-id="7fa19-201">Se puede establecer una hora de expiración <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>absoluta con.</span><span class="sxs-lookup"><span data-stu-id="7fa19-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="7fa19-202">Para crear una cookie persistente, `IsPersistent` también se debe establecer.</span><span class="sxs-lookup"><span data-stu-id="7fa19-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="7fa19-203">De lo contrario, la cookie se crea con una duración basada en sesión y puede expirar antes o después del vale de autenticación que contiene.</span><span class="sxs-lookup"><span data-stu-id="7fa19-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="7fa19-204">Cuando `ExpiresUtc` se establece, reemplaza el valor de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opción de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si se establece.</span><span class="sxs-lookup"><span data-stu-id="7fa19-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="7fa19-205">El siguiente fragmento de código crea una identidad y la cookie correspondiente que dura 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="7fa19-206">Esto omite cualquier configuración de expiración variable previamente configurada.</span><span class="sxs-lookup"><span data-stu-id="7fa19-206">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7fa19-207">ASP.NET Core Identity es un completo proveedor de autenticación completo para crear y mantener inicios de sesión.</span><span class="sxs-lookup"><span data-stu-id="7fa19-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="7fa19-208">Sin embargo, se puede usar un proveedor de autenticación de autenticación basada en cookies sin ASP.NET Core identidad.</span><span class="sxs-lookup"><span data-stu-id="7fa19-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="7fa19-209">Para obtener más información, consulta <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="7fa19-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="7fa19-210">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7fa19-210">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7fa19-211">Para fines de demostración en la aplicación de ejemplo, la cuenta de usuario para el usuario hipotético, María Rodriguez, está codificada en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7fa19-212">Use la dirección `maria.rodriguez@contoso.com` de **correo electrónico** y cualquier contraseña para iniciar sesión en el usuario.</span><span class="sxs-lookup"><span data-stu-id="7fa19-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="7fa19-213">El usuario se autentica en el `AuthenticateUser` método del archivo *pages/Account/login. cshtml. CS* .</span><span class="sxs-lookup"><span data-stu-id="7fa19-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7fa19-214">En un ejemplo real, el usuario se autenticaría en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="7fa19-215">Configuración</span><span class="sxs-lookup"><span data-stu-id="7fa19-215">Configuration</span></span>

<span data-ttu-id="7fa19-216">Si la aplicación no usa el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), cree una referencia de paquete en el archivo de proyecto para el paquete [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="7fa19-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="7fa19-217">En el `Startup.ConfigureServices` método, cree el servicio de middleware de autenticación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> con <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> los métodos y:</span><span class="sxs-lookup"><span data-stu-id="7fa19-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7fa19-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>se pasa `AddAuthentication` a establece el esquema de autenticación predeterminado para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="7fa19-219">`AuthenticationScheme`resulta útil cuando hay varias instancias de autenticación de cookies y se desea [autorizar con un esquema específico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="7fa19-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7fa19-220">Si se establece en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) , se proporciona el valor "cookies" para el esquema. `AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="7fa19-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7fa19-221">Puede proporcionar cualquier valor de cadena que distinga el esquema.</span><span class="sxs-lookup"><span data-stu-id="7fa19-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="7fa19-222">El esquema de autenticación de la aplicación es diferente del esquema de autenticación de cookies de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="7fa19-223">Cuando no se proporciona un esquema de autenticación <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>de cookies `CookieAuthenticationDefaults.AuthenticationScheme` a, USA ("cookies").</span><span class="sxs-lookup"><span data-stu-id="7fa19-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="7fa19-224">La propiedad de la <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> cookie de autenticación está `true` establecida de forma predeterminada en.</span><span class="sxs-lookup"><span data-stu-id="7fa19-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="7fa19-225">Las cookies de autenticación se permiten cuando un visitante del sitio no ha dado su consentimiento a la recopilación de datos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="7fa19-226">Para obtener más información, consulta <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="7fa19-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="7fa19-227">En el `Startup.Configure` método, `UseAuthentication` llame al método para invocar el middleware de autenticación que `HttpContext.User` establece la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7fa19-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="7fa19-228">Llame al `UseAuthentication` método antes de `UseMvcWithDefaultRoute` llamar `UseMvc`a o a:</span><span class="sxs-lookup"><span data-stu-id="7fa19-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="7fa19-229">La <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> clase se utiliza para configurar las opciones del proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="7fa19-230">Establezca `CookieAuthenticationOptions` en la configuración del servicio para la autenticación `Startup.ConfigureServices` en el método:</span><span class="sxs-lookup"><span data-stu-id="7fa19-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="7fa19-231">Middleware de directiva de cookies</span><span class="sxs-lookup"><span data-stu-id="7fa19-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="7fa19-232">El [middleware de directiva de cookies](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) habilita las funcionalidades de directiva de cookies.</span><span class="sxs-lookup"><span data-stu-id="7fa19-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="7fa19-233">Agregar el middleware a la canalización de procesamiento de la&mdash;aplicación es dependiente del orden. solo afecta a los componentes de nivel inferior registrados en la canalización.</span><span class="sxs-lookup"><span data-stu-id="7fa19-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="7fa19-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> proporcionado al middleware de la Directiva de cookies para controlar las características globales del procesamiento de cookies y enlazar los controladores de procesamiento de cookies cuando se anexan o eliminan cookies.</span><span class="sxs-lookup"><span data-stu-id="7fa19-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="7fa19-235">El valor <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> predeterminado es `SameSiteMode.Lax` permitir la autenticación de OAuth2.</span><span class="sxs-lookup"><span data-stu-id="7fa19-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="7fa19-236">Para aplicar estrictamente una directiva del mismo sitio de `SameSiteMode.Strict`, `MinimumSameSitePolicy`establezca.</span><span class="sxs-lookup"><span data-stu-id="7fa19-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="7fa19-237">Aunque esta configuración interrumpe OAuth2 y otros esquemas de autenticación entre orígenes, eleva el nivel de seguridad de las cookies para otros tipos de aplicaciones que no se basan en el procesamiento de solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="7fa19-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="7fa19-238">La configuración de middleware de la `MinimumSameSitePolicy` Directiva de cookies para `Cookie.SameSite` puede `CookieAuthenticationOptions` afectar a la configuración de en la configuración de acuerdo con la matriz siguiente.</span><span class="sxs-lookup"><span data-stu-id="7fa19-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="7fa19-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7fa19-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="7fa19-240">Cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="7fa19-240">Cookie.SameSite</span></span> | <span data-ttu-id="7fa19-241">Valor de cookie. SameSite resultante</span><span class="sxs-lookup"><span data-stu-id="7fa19-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="7fa19-242">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-242">SameSiteMode.None</span></span>     | <span data-ttu-id="7fa19-243">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-243">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-244">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-245">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-246">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-246">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-247">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-248">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7fa19-249">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="7fa19-250">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-250">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-251">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-252">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-253">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-254">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-255">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7fa19-256">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="7fa19-257">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7fa19-257">SameSiteMode.None</span></span><br><span data-ttu-id="7fa19-258">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7fa19-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="7fa19-259">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="7fa19-260">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="7fa19-261">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="7fa19-262">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7fa19-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="7fa19-263">Crear una cookie de autenticación</span><span class="sxs-lookup"><span data-stu-id="7fa19-263">Create an authentication cookie</span></span>

<span data-ttu-id="7fa19-264">Para crear una cookie que contiene información de usuario, <xref:System.Security.Claims.ClaimsPrincipal>construya un.</span><span class="sxs-lookup"><span data-stu-id="7fa19-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="7fa19-265">La información del usuario se serializa y se almacena en la cookie.</span><span class="sxs-lookup"><span data-stu-id="7fa19-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="7fa19-266">Cree un <xref:System.Security.Claims.ClaimsIdentity> con los s <xref:System.Security.Claims.Claim>necesarios y llame <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> al usuario para iniciar sesión:</span><span class="sxs-lookup"><span data-stu-id="7fa19-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="7fa19-267">`SignInAsync`crea una cookie cifrada y la agrega a la respuesta actual.</span><span class="sxs-lookup"><span data-stu-id="7fa19-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="7fa19-268">Si `AuthenticationScheme` no se especifica, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="7fa19-269">El sistema de [protección de datos](xref:security/data-protection/using-data-protection) de ASP.net Core se usa para el cifrado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="7fa19-270">En el caso de una aplicación hospedada en varios equipos, equilibrio de carga entre aplicaciones o uso de una granja de servidores Web, [Configure la protección de datos](xref:security/data-protection/configuration/overview) para que use el mismo anillo de claves e identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="7fa19-271">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="7fa19-271">Sign out</span></span>

<span data-ttu-id="7fa19-272">Para cerrar la sesión del usuario actual y eliminar su cookie, <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>llame a:</span><span class="sxs-lookup"><span data-stu-id="7fa19-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="7fa19-273">Si `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookies") no se usa como esquema (por ejemplo, "ContosoCookie"), proporcione el esquema que se usa al configurar el proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="7fa19-274">De lo contrario, se usa el esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7fa19-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="7fa19-275">Reaccionar a los cambios de back-end</span><span class="sxs-lookup"><span data-stu-id="7fa19-275">React to back-end changes</span></span>

<span data-ttu-id="7fa19-276">Una vez creada una cookie, ésta es la única fuente de identidad.</span><span class="sxs-lookup"><span data-stu-id="7fa19-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="7fa19-277">Si una cuenta de usuario está deshabilitada en los sistemas de servidor:</span><span class="sxs-lookup"><span data-stu-id="7fa19-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="7fa19-278">El sistema de autenticación de cookies de la aplicación continúa procesando solicitudes basadas en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="7fa19-279">El usuario permanecerá conectado en la aplicación siempre y cuando la cookie de autenticación sea válida.</span><span class="sxs-lookup"><span data-stu-id="7fa19-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="7fa19-280">El <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento se puede utilizar para interceptar e invalidar la validación de la identidad de la cookie.</span><span class="sxs-lookup"><span data-stu-id="7fa19-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="7fa19-281">La validación de la cookie en cada solicitud mitiga el riesgo de que los usuarios revocados accedan a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="7fa19-282">Un enfoque para la validación de cookies se basa en realizar un seguimiento de cuándo cambia la base de datos de usuario.</span><span class="sxs-lookup"><span data-stu-id="7fa19-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="7fa19-283">Si no se ha cambiado la base de datos desde que se emitió la cookie del usuario, no es necesario volver a autenticar al usuario si la cookie sigue siendo válida.</span><span class="sxs-lookup"><span data-stu-id="7fa19-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="7fa19-284">En la aplicación de ejemplo, la base de datos `IUserRepository` se implementa en `LastChanged` y almacena un valor.</span><span class="sxs-lookup"><span data-stu-id="7fa19-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="7fa19-285">Cuando se actualiza un usuario en la base de datos `LastChanged` , el valor se establece en la hora actual.</span><span class="sxs-lookup"><span data-stu-id="7fa19-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="7fa19-286">Para invalidar una cookie cuando la base de datos cambie según el `LastChanged` valor, cree la cookie con una `LastChanged` solicitud que contenga el `LastChanged` valor actual de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="7fa19-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="7fa19-287">Para implementar una invalidación para `ValidatePrincipal` el evento, escriba un método con la siguiente firma en una clase que derive de <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="7fa19-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="7fa19-288">El siguiente es un ejemplo de implementación `CookieAuthenticationEvents`de:</span><span class="sxs-lookup"><span data-stu-id="7fa19-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="7fa19-289">Registre la instancia de eventos durante el registro del servicio `Startup.ConfigureServices` de cookies en el método.</span><span class="sxs-lookup"><span data-stu-id="7fa19-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7fa19-290">Proporcione un [registro de servicio de ámbito](xref:fundamentals/dependency-injection#service-lifetimes) para `CustomCookieAuthenticationEvents` la clase:</span><span class="sxs-lookup"><span data-stu-id="7fa19-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="7fa19-291">Considere una situación en la que el nombre del usuario se&mdash;actualice una decisión que no afecte a la seguridad de ningún modo.</span><span class="sxs-lookup"><span data-stu-id="7fa19-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="7fa19-292">Si desea actualizar de la entidad de seguridad de usuario de una no destructiva `context.ReplacePrincipal` , llame a `context.ShouldRenew` y establezca `true`la propiedad en.</span><span class="sxs-lookup"><span data-stu-id="7fa19-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="7fa19-293">El enfoque descrito aquí se desencadena en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="7fa19-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="7fa19-294">La validación de las cookies de autenticación para todos los usuarios de cada solicitud puede producir una gran penalización del rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7fa19-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="7fa19-295">Cookies persistentes</span><span class="sxs-lookup"><span data-stu-id="7fa19-295">Persistent cookies</span></span>

<span data-ttu-id="7fa19-296">Puede que desee que la cookie se conserve entre sesiones del explorador.</span><span class="sxs-lookup"><span data-stu-id="7fa19-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="7fa19-297">Esta persistencia solo se debe habilitar con el consentimiento explícito del usuario con una casilla "Recordarme" al iniciar sesión o un mecanismo similar.</span><span class="sxs-lookup"><span data-stu-id="7fa19-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="7fa19-298">El siguiente fragmento de código crea una identidad y la cookie correspondiente que sobrevive a través de los cierres del explorador.</span><span class="sxs-lookup"><span data-stu-id="7fa19-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="7fa19-299">Se respetan los valores de expiración que se hayan configurado previamente.</span><span class="sxs-lookup"><span data-stu-id="7fa19-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="7fa19-300">Si la cookie expira mientras se cierra el explorador, el explorador borra la cookie una vez que se reinicia.</span><span class="sxs-lookup"><span data-stu-id="7fa19-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="7fa19-301">Se <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>establece enen:`true`</span><span class="sxs-lookup"><span data-stu-id="7fa19-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="7fa19-302">Expiración absoluta de cookies</span><span class="sxs-lookup"><span data-stu-id="7fa19-302">Absolute cookie expiration</span></span>

<span data-ttu-id="7fa19-303">Se puede establecer una hora de expiración <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>absoluta con.</span><span class="sxs-lookup"><span data-stu-id="7fa19-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="7fa19-304">Para crear una cookie persistente, `IsPersistent` también se debe establecer.</span><span class="sxs-lookup"><span data-stu-id="7fa19-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="7fa19-305">De lo contrario, la cookie se crea con una duración basada en sesión y puede expirar antes o después del vale de autenticación que contiene.</span><span class="sxs-lookup"><span data-stu-id="7fa19-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="7fa19-306">Cuando `ExpiresUtc` se establece, reemplaza el valor de la <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opción de <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, si se establece.</span><span class="sxs-lookup"><span data-stu-id="7fa19-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="7fa19-307">El siguiente fragmento de código crea una identidad y la cookie correspondiente que dura 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="7fa19-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="7fa19-308">Esto omite cualquier configuración de expiración variable previamente configurada.</span><span class="sxs-lookup"><span data-stu-id="7fa19-308">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7fa19-309">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7fa19-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="7fa19-310">Comprobaciones de roles basadas en directivas</span><span class="sxs-lookup"><span data-stu-id="7fa19-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
