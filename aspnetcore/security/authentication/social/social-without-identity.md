---
title: La autenticación de Facebook, Google y proveedores externos sin ASP.NET Core identidad
author: rick-anderson
description: Explicación del uso de Facebook, Google, Twitter, etc. autenticación de usuarios de cuentas sin ASP.NET Core identidad.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999899"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="057d4-103">Usar la autenticación de proveedor de inicio de sesión social sin ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="057d4-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="057d4-104"><xref:security/authentication/social/index> describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="057d4-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="057d4-105">El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="057d4-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="057d4-106">Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad.</span><span class="sxs-lookup"><span data-stu-id="057d4-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="057d4-107">Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.</span><span class="sxs-lookup"><span data-stu-id="057d4-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="057d4-108">Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="057d4-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="057d4-109">El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google.</span><span class="sxs-lookup"><span data-stu-id="057d4-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="057d4-110">Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="057d4-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="057d4-111">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="057d4-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="057d4-112">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="057d4-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="057d4-113">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="057d4-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="057d4-114">Otros proveedores</span><span class="sxs-lookup"><span data-stu-id="057d4-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="057d4-115">Configuración</span><span class="sxs-lookup"><span data-stu-id="057d4-115">Configuration</span></span>

<span data-ttu-id="057d4-116">En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> y <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:</span><span class="sxs-lookup"><span data-stu-id="057d4-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

<span data-ttu-id="057d4-117">La llamada a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> establece la @no__t de la aplicación-1.</span><span class="sxs-lookup"><span data-stu-id="057d4-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="057d4-118">El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="057d4-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="057d4-119">Al establecer el @no__t de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="057d4-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="057d4-120">Al establecer el @no__t de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para que use Google como esquema predeterminado para las llamadas a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="057d4-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="057d4-121">`DefaultChallengeScheme` reemplaza a `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="057d4-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="057d4-122">Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> para obtener más propiedades que invalide `DefaultScheme` cuando se establezca.</span><span class="sxs-lookup"><span data-stu-id="057d4-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="057d4-123">En `Startup.Configure`, llame a `UseAuthentication` y @no__t 2 para establecer la propiedad `HttpContext.User` y ejecutar el middleware de autorización para las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="057d4-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="057d4-124">Llame a los métodos `UseAuthentication` y `UseAuthorization` antes de llamar a `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="057d4-124">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

<span data-ttu-id="057d4-125">Para obtener más información sobre los esquemas de autenticación y la autenticación de cookies, vea <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="057d4-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="057d4-126">Aplicar autorización</span><span class="sxs-lookup"><span data-stu-id="057d4-126">Apply authorization</span></span>

<span data-ttu-id="057d4-127">Pruebe la configuración de autenticación de la aplicación aplicando el atributo `AuthorizeAttribute` a un controlador, acción o página.</span><span class="sxs-lookup"><span data-stu-id="057d4-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="057d4-128">El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:</span><span class="sxs-lookup"><span data-stu-id="057d4-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="057d4-129">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="057d4-129">Sign out</span></span>

<span data-ttu-id="057d4-130">Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="057d4-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="057d4-131">El siguiente código agrega un controlador de página `Logout` a la página de *Índice* :</span><span class="sxs-lookup"><span data-stu-id="057d4-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="057d4-132">Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="057d4-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="057d4-133">El @no__t de la aplicación-0 de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.</span><span class="sxs-lookup"><span data-stu-id="057d4-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="057d4-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="057d4-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="057d4-135"><xref:security/authentication/social/index> describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="057d4-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="057d4-136">El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="057d4-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="057d4-137">Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad.</span><span class="sxs-lookup"><span data-stu-id="057d4-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="057d4-138">Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.</span><span class="sxs-lookup"><span data-stu-id="057d4-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="057d4-139">Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="057d4-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="057d4-140">El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google.</span><span class="sxs-lookup"><span data-stu-id="057d4-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="057d4-141">Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="057d4-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="057d4-142">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="057d4-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="057d4-143">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="057d4-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="057d4-144">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="057d4-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="057d4-145">Otros proveedores</span><span class="sxs-lookup"><span data-stu-id="057d4-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="057d4-146">Configuración</span><span class="sxs-lookup"><span data-stu-id="057d4-146">Configuration</span></span>

<span data-ttu-id="057d4-147">En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos `AddAuthentication`, `AddCookie` y `AddGoogle`:</span><span class="sxs-lookup"><span data-stu-id="057d4-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="057d4-148">La llamada a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) establece el [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="057d4-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="057d4-149">El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="057d4-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="057d4-150">Al establecer el @no__t de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="057d4-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="057d4-151">Al establecer el @no__t de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para que use Google como esquema predeterminado para las llamadas a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="057d4-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="057d4-152">`DefaultChallengeScheme` reemplaza a `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="057d4-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="057d4-153">Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> para obtener más propiedades que invalide `DefaultScheme` cuando se establezca.</span><span class="sxs-lookup"><span data-stu-id="057d4-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="057d4-154">En el método `Configure`, llame al método `UseAuthentication` para invocar el middleware de autenticación que establece la propiedad `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="057d4-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="057d4-155">Llame al método `UseAuthentication` antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="057d4-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="057d4-156">Para obtener más información sobre los esquemas de autenticación y la autenticación de cookies, vea <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="057d4-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="057d4-157">Aplicar autorización</span><span class="sxs-lookup"><span data-stu-id="057d4-157">Apply authorization</span></span>

<span data-ttu-id="057d4-158">Pruebe la configuración de autenticación de la aplicación aplicando el atributo `AuthorizeAttribute` a un controlador, acción o página.</span><span class="sxs-lookup"><span data-stu-id="057d4-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="057d4-159">El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:</span><span class="sxs-lookup"><span data-stu-id="057d4-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="057d4-160">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="057d4-160">Sign out</span></span>

<span data-ttu-id="057d4-161">Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="057d4-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="057d4-162">El siguiente código agrega un controlador de página `Logout` a la página de *Índice* :</span><span class="sxs-lookup"><span data-stu-id="057d4-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="057d4-163">Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="057d4-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="057d4-164">El @no__t de la aplicación-0 de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.</span><span class="sxs-lookup"><span data-stu-id="057d4-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="057d4-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="057d4-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
