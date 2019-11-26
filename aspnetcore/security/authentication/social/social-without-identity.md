---
title: La autenticación de Facebook, Google y proveedores externos sin ASP.NET Core identidad
author: rick-anderson
description: Explicación del uso de Facebook, Google, Twitter, etc. autenticación de usuarios de cuentas sin ASP.NET Core identidad.
ms.author: riande
ms.date: 11/19/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 680ea091dcc5ed7f94879b5d277e8be7e5abeb7b
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289118"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="247c4-103">Usar la autenticación de proveedor de inicio de sesión social sin ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="247c4-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="247c4-104">en <xref:security/authentication/social/index> se describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="247c4-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="247c4-105">El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="247c4-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="247c4-106">Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad.</span><span class="sxs-lookup"><span data-stu-id="247c4-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="247c4-107">Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.</span><span class="sxs-lookup"><span data-stu-id="247c4-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="247c4-108">Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="247c4-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="247c4-109">El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google.</span><span class="sxs-lookup"><span data-stu-id="247c4-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="247c4-110">Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="247c4-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="247c4-111">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="247c4-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="247c4-112">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="247c4-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="247c4-113">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="247c4-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="247c4-114">Otros proveedores</span><span class="sxs-lookup"><span data-stu-id="247c4-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="247c4-115">Configuración</span><span class="sxs-lookup"><span data-stu-id="247c4-115">Configuration</span></span>

<span data-ttu-id="247c4-116">En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>y <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:</span><span class="sxs-lookup"><span data-stu-id="247c4-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<span data-ttu-id="247c4-117">La llamada a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> establece el <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="247c4-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="247c4-118">El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación de `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="247c4-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="247c4-119">Al establecer la `DefaultScheme` de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="247c4-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="247c4-120">Al establecer la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para usar Google como esquema predeterminado para las llamadas a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="247c4-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="247c4-121">`DefaultChallengeScheme` invalida `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="247c4-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="247c4-122">Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propiedades adicionales que invalidan `DefaultScheme` cuando se establecen.</span><span class="sxs-lookup"><span data-stu-id="247c4-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="247c4-123">En `Startup.Configure`, llame a `UseAuthentication` y `UseAuthorization` entre las llamadas a `UseRouting` y `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="247c4-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` between calling `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="247c4-124">Esto establece la propiedad `HttpContext.User` y ejecuta el middleware de autorización para las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="247c4-124">This sets the `HttpContext.User` property and runs the Authorization Middleware for requests:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

<span data-ttu-id="247c4-125">Para obtener más información acerca de los esquemas de autenticación y la autenticación de cookies, consulte <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="247c4-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="247c4-126">Aplicar autorización</span><span class="sxs-lookup"><span data-stu-id="247c4-126">Apply authorization</span></span>

<span data-ttu-id="247c4-127">Pruebe la configuración de autenticación de la aplicación aplicando el `AuthorizeAttribute` atributo a un controlador, acción o página.</span><span class="sxs-lookup"><span data-stu-id="247c4-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="247c4-128">El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:</span><span class="sxs-lookup"><span data-stu-id="247c4-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="247c4-129">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="247c4-129">Sign out</span></span>

<span data-ttu-id="247c4-130">Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="247c4-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="247c4-131">En el código siguiente se agrega un controlador de página de `Logout` a la página de *Índice* :</span><span class="sxs-lookup"><span data-stu-id="247c4-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="247c4-132">Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="247c4-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="247c4-133">La `DefaultScheme` de la aplicación de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.</span><span class="sxs-lookup"><span data-stu-id="247c4-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="247c4-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="247c4-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="247c4-135">en <xref:security/authentication/social/index> se describe cómo permitir a los usuarios iniciar sesión con OAuth 2,0 con las credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="247c4-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="247c4-136">El enfoque descrito en ese tema incluye ASP.NET Core identidad como proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="247c4-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="247c4-137">Este ejemplo muestra cómo utilizar un proveedor de autenticación externo **sin** ASP.net Core identidad.</span><span class="sxs-lookup"><span data-stu-id="247c4-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="247c4-138">Esto resulta útil para las aplicaciones que no requieren todas las características de ASP.NET Core identidad, pero que aún requieren la integración con un proveedor de autenticación externo de confianza.</span><span class="sxs-lookup"><span data-stu-id="247c4-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="247c4-139">Este ejemplo utiliza la [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="247c4-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="247c4-140">El uso de la autenticación de Google desplaza muchas de las complejidades de la administración del proceso de inicio de sesión en Google.</span><span class="sxs-lookup"><span data-stu-id="247c4-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="247c4-141">Para integrar con otro proveedor de autenticación externo, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="247c4-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="247c4-142">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="247c4-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="247c4-143">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="247c4-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="247c4-144">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="247c4-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="247c4-145">Otros proveedores</span><span class="sxs-lookup"><span data-stu-id="247c4-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="247c4-146">Configuración</span><span class="sxs-lookup"><span data-stu-id="247c4-146">Configuration</span></span>

<span data-ttu-id="247c4-147">En el método `ConfigureServices`, configure los esquemas de autenticación de la aplicación con los métodos `AddAuthentication`, `AddCookie`y `AddGoogle`:</span><span class="sxs-lookup"><span data-stu-id="247c4-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="247c4-148">La llamada a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) establece el [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="247c4-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="247c4-149">El `DefaultScheme` es el esquema predeterminado utilizado por los siguientes métodos de extensión de autenticación de `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="247c4-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="247c4-150">Al establecer la `DefaultScheme` de la aplicación en [CookieAuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), se configura la aplicación para que use cookies como esquema predeterminado para estos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="247c4-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="247c4-151">Al establecer la <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> de la aplicación en [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), se configura la aplicación para usar Google como esquema predeterminado para las llamadas a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="247c4-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="247c4-152">`DefaultChallengeScheme` invalida `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="247c4-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="247c4-153">Vea <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propiedades adicionales que invalidan `DefaultScheme` cuando se establecen.</span><span class="sxs-lookup"><span data-stu-id="247c4-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="247c4-154">En el método `Configure`, llame al método `UseAuthentication` para invocar el middleware de autenticación que establece la propiedad `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="247c4-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="247c4-155">Llame al método `UseAuthentication` antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="247c4-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

<span data-ttu-id="247c4-156">Para obtener más información acerca de los esquemas de autenticación y la autenticación de cookies, consulte <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="247c4-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="247c4-157">Aplicar autorización</span><span class="sxs-lookup"><span data-stu-id="247c4-157">Apply authorization</span></span>

<span data-ttu-id="247c4-158">Pruebe la configuración de autenticación de la aplicación aplicando el `AuthorizeAttribute` atributo a un controlador, acción o página.</span><span class="sxs-lookup"><span data-stu-id="247c4-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="247c4-159">El código siguiente limita el acceso a la página de *privacidad* a los usuarios que se han autenticado:</span><span class="sxs-lookup"><span data-stu-id="247c4-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="247c4-160">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="247c4-160">Sign out</span></span>

<span data-ttu-id="247c4-161">Para cerrar la sesión del usuario actual y eliminar su cookie, llame a [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span><span class="sxs-lookup"><span data-stu-id="247c4-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="247c4-162">En el código siguiente se agrega un controlador de página de `Logout` a la página de *Índice* :</span><span class="sxs-lookup"><span data-stu-id="247c4-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="247c4-163">Observe que la llamada a `SignOutAsync` no especifica un esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="247c4-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="247c4-164">La `DefaultScheme` de la aplicación de `CookieAuthenticationDefaults.AuthenticationScheme` se usa como retroceso.</span><span class="sxs-lookup"><span data-stu-id="247c4-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="247c4-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="247c4-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
