---
title: Facebook, Google y la autenticación de proveedor externo sin ASP.NET Core Identity
author: rick-anderson
description: Explicación del uso de Facebook, Google, Twitter, autenticación de usuario de cuenta etc. sin ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557654"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="9d0d7-103">Usar la autenticación del proveedor de inicio de sesión social sin ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="9d0d7-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="9d0d7-104"><xref:security/authentication/social/index> Describe cómo habilitar usuarios iniciar sesión mediante OAuth 2.0 con las credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="9d0d7-105">El enfoque descrito en este tema incluye ASP.NET Core Identity como proveedor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="9d0d7-106">Este ejemplo muestra cómo usar el proveedor de autenticación externo **sin** ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="9d0d7-107">Esto es útil para las aplicaciones que no requieren todas las características de ASP.NET Core Identity, pero seguirá necesitan la integración con un proveedor de autenticación externo de confianza.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="9d0d7-108">Este ejemplo utiliza [autenticación de Google](xref:security/authentication/google-logins) para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="9d0d7-109">Autenticación con Google desplaza muchas de las complejidades de administrar el proceso de inicio de sesión de Google.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="9d0d7-110">Para integrar con un proveedor de autenticación externo diferente, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="9d0d7-111">Autenticación con Facebook</span><span class="sxs-lookup"><span data-stu-id="9d0d7-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="9d0d7-112">Autenticación con Microsoft</span><span class="sxs-lookup"><span data-stu-id="9d0d7-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="9d0d7-113">Autenticación con Twitter</span><span class="sxs-lookup"><span data-stu-id="9d0d7-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="9d0d7-114">Otros proveedores</span><span class="sxs-lookup"><span data-stu-id="9d0d7-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="9d0d7-115">Configuración</span><span class="sxs-lookup"><span data-stu-id="9d0d7-115">Configuration</span></span>

<span data-ttu-id="9d0d7-116">En el `ConfigureServices` método, configure los esquemas de autenticación de la aplicación con el `AddAuthentication`, `AddCookie` y `AddGoogle` métodos:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="9d0d7-117">La llamada a [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) establece la aplicación [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="9d0d7-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="9d0d7-118">El `DefaultScheme` es el esquema predeterminado para las siguientes `HttpContext` métodos de extensión de autenticación:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="9d0d7-119">Configuración de la aplicación `DefaultScheme` a [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configura la aplicación para usar las Cookies que el esquema predeterminado para estos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="9d0d7-120">Configuración de la aplicación <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> a [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configura la aplicación para usar Google como el esquema predeterminado para las llamadas a `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="9d0d7-121">`DefaultChallengeScheme` invalida `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="9d0d7-122">Consulte <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> de propiedades adicionales que reemplazan `DefaultScheme` cuando se establece.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="9d0d7-123">En el `Configure` método, llame a la `UseAuthentication` método para invocar el Middleware de autenticación que establece el `HttpContext.User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="9d0d7-124">Llame a la `UseAuthentication` método antes de llamar a `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="9d0d7-125">Para obtener más información acerca de los esquemas de autenticación y autenticación con cookies, consulte <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-basic-authorization"></a><span data-ttu-id="9d0d7-126">Aplicar la autorización básica</span><span class="sxs-lookup"><span data-stu-id="9d0d7-126">Applying basic authorization</span></span>

<span data-ttu-id="9d0d7-127">Probar la configuración de autenticación de la aplicación aplicando la `AuthorizeAttribute` atributo a un controlador, acción o página.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="9d0d7-128">El siguiente código limita el acceso a la *privacidad* página a los usuarios que han sido autenticados:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="9d0d7-129">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="9d0d7-129">Sign out</span></span>

<span data-ttu-id="9d0d7-130">Para cerrar la sesión del usuario actual y eliminar sus cookies, llame a [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="9d0d7-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="9d0d7-131">El código siguiente agrega un `Logout` controlador de páginas la *índice* página:</span><span class="sxs-lookup"><span data-stu-id="9d0d7-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="9d0d7-132">Tenga en cuenta que la llamada a `SignOutAsync` no especifica un esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="9d0d7-133">La aplicación `DefaultScheme` de `CookieAuthenticationDefaults.AuthenticationScheme` se utiliza como un retroceso.</span><span class="sxs-lookup"><span data-stu-id="9d0d7-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d0d7-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9d0d7-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
