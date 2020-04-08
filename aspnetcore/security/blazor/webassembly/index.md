---
title: Protección de WebAssembly de Blazor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo proteger aplicaciones WebAssemlby de Blazor como aplicaciones de página única (SPA).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: be286d770cd8d6e5cf7885b91be8654f74ffd743
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80538982"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="ac265-103">Protección de WebAssembly de Blazor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac265-103">Secure ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="ac265-104">Por [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="ac265-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="ac265-105">Las aplicaciones WebAssembly de Blazor se protegen de la misma manera que las aplicaciones de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="ac265-105">Blazor WebAssembly apps are secured in the same manner as Single Page Applications (SPAs).</span></span> <span data-ttu-id="ac265-106">Hay varios métodos para autenticar a los usuarios en las SPA, pero el enfoque más común y completo consiste en usar una implementación basada en el [protocolo OAuth 2.0](https://oauth.net/), como [Open ID Connect (OIDC)](https://openid.net/connect/).</span><span class="sxs-lookup"><span data-stu-id="ac265-106">There are several approaches for authenticating users to SPAs, but the most common and comprehensive approach is to use an implementation based on the [OAuth 2.0 protocol](https://oauth.net/), such as [Open ID Connect (OIDC)](https://openid.net/connect/).</span></span>

## <a name="authentication-library"></a><span data-ttu-id="ac265-107">Biblioteca de autenticación</span><span class="sxs-lookup"><span data-stu-id="ac265-107">Authentication library</span></span>

<span data-ttu-id="ac265-108">WebAssembly de Blazor permite autenticar y autorizar aplicaciones mediante OIDC a través de la biblioteca `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="ac265-108">Blazor WebAssembly supports authenticating and authorizing apps using OIDC via the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library.</span></span> <span data-ttu-id="ac265-109">La biblioteca proporciona un conjunto de primitivas para la autenticación sin problemas en back-ends de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac265-109">The library provides a set of primitives for seamlessly authenticating against ASP.NET Core backends.</span></span> <span data-ttu-id="ac265-110">La biblioteca integra ASP.NET Core Identity con compatibilidad con la autorización de API basada en [Identity Server](https://identityserver.io/).</span><span class="sxs-lookup"><span data-stu-id="ac265-110">The library integrates ASP.NET Core Identity with API authorization support built on top of [Identity Server](https://identityserver.io/).</span></span> <span data-ttu-id="ac265-111">La biblioteca puede realizar la autenticación sobre cualquier proveedor de identidades (IP) de terceros que admita OIDC, que se denominan proveedores de OpenID (OP).</span><span class="sxs-lookup"><span data-stu-id="ac265-111">The library can authenticate against any third-party Identity Provider (IP) that supports OIDC, which are called OpenID Providers (OP).</span></span>

<span data-ttu-id="ac265-112">La compatibilidad con la autenticación en WebAssembly de Blazor se basa en la biblioteca *oidc-client.js*, que se usa para administrar los detalles del protocolo de autenticación subyacente.</span><span class="sxs-lookup"><span data-stu-id="ac265-112">The authentication support in Blazor WebAssembly is built on top of the *oidc-client.js* library, which is used to handle the underlying authentication protocol details.</span></span>

<span data-ttu-id="ac265-113">Existen otras opciones para la autenticación de las SPA, como el uso de cookies de SameSite.</span><span class="sxs-lookup"><span data-stu-id="ac265-113">Other options for authenticating SPAs exist, such as the use of SameSite cookies.</span></span> <span data-ttu-id="ac265-114">Sin embargo, el diseño de ingeniería de WebAssembly de Blazor se limita a OAuth y OIDC como mejor opción para la autenticación en las aplicaciones WebAssembly de Blazor.</span><span class="sxs-lookup"><span data-stu-id="ac265-114">However, the engineering design of Blazor WebAssembly is settled on OAuth and OIDC as the best option for authentication in Blazor WebAssembly apps.</span></span> <span data-ttu-id="ac265-115">Se ha elegido la [autenticación basada en tokens](xref:security/anti-request-forgery#token-based-authentication) basada en [JSON Web Token (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) antes que la [autenticación basada en cookies](xref:security/anti-request-forgery#cookie-based-authentication) por razones funcionales y de seguridad:</span><span class="sxs-lookup"><span data-stu-id="ac265-115">[Token-based authentication](xref:security/anti-request-forgery#token-based-authentication) based on [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) was chosen over [cookie-based authentication](xref:security/anti-request-forgery#cookie-based-authentication) for functional and security reasons:</span></span>

* <span data-ttu-id="ac265-116">El uso de un protocolo basado en tokens ofrece una superficie expuesta a ataques más pequeña, ya que los tokens no se envían en todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="ac265-116">Using a token-based protocol offers a smaller attack surface area, as the tokens aren't sent in all requests.</span></span>
* <span data-ttu-id="ac265-117">Los puntos de conexión de servidor no requieren protección contra la [Falsificación de solicitudes entre sitios (CSRF)](xref:security/anti-request-forgery), ya que los tokens se envían de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="ac265-117">Server endpoints don't require protection against [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) because the tokens are sent explicitly.</span></span> <span data-ttu-id="ac265-118">Esto permite hospedar aplicaciones WebAssembly de Blazor junto con aplicaciones MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ac265-118">This allows you to host Blazor WebAssembly apps alongside MVC or Razor pages apps.</span></span>
* <span data-ttu-id="ac265-119">Los tokens tienen permisos más restringidos que las cookies.</span><span class="sxs-lookup"><span data-stu-id="ac265-119">Tokens have narrower permissions than cookies.</span></span> <span data-ttu-id="ac265-120">Por ejemplo, los tokens no se pueden usar para administrar la cuenta de usuario o cambiar su contraseña a menos que esa funcionalidad se implemente de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="ac265-120">For example, tokens can't be used to manage the user account or change a user's password unless such functionality is explicitly implemented.</span></span>
* <span data-ttu-id="ac265-121">Los tokens tienen una duración corta, una hora de forma predeterminada, lo que limita la ventana de ataque.</span><span class="sxs-lookup"><span data-stu-id="ac265-121">Tokens have a short lifetime, one hour by default, which limits the attack window.</span></span> <span data-ttu-id="ac265-122">Los tokens también se pueden revocar en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="ac265-122">Tokens can also be revoked at any time.</span></span>
* <span data-ttu-id="ac265-123">Los JWT independientes ofrecen garantías al cliente y al servidor sobre el proceso de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac265-123">Self-contained JWTs offer guarantees to the client and server about the authentication process.</span></span> <span data-ttu-id="ac265-124">Por ejemplo, un cliente tiene los medios para detectar y validar que los tokens que recibe son legítimos y se han emitido como parte de un proceso de autenticación determinado.</span><span class="sxs-lookup"><span data-stu-id="ac265-124">For example, a client has the means to detect and validate that the tokens it receives are legitimate and were emitted as part of a given authentication process.</span></span> <span data-ttu-id="ac265-125">Si un tercero intenta cambiar un token en medio del proceso de autenticación, el cliente puede detectar el token cambiado y evitar usarlo.</span><span class="sxs-lookup"><span data-stu-id="ac265-125">If a third party attempts to switch a token in the middle of the authentication process, the client can detect the switched token and avoid using it.</span></span>
* <span data-ttu-id="ac265-126">Los tokens con OAuth y OIDC no se basan en que el agente de usuario se comporte correctamente para asegurarse de que la aplicación sea segura.</span><span class="sxs-lookup"><span data-stu-id="ac265-126">Tokens with OAuth and OIDC don't rely on the user agent behaving correctly to ensure that the app is secure.</span></span>
* <span data-ttu-id="ac265-127">Los protocolos basados en tokens, como OAuth y OIDC, permiten autenticar y autorizar aplicaciones independientes y hospedadas con el mismo conjunto de características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ac265-127">Token-based protocols, such as OAuth and OIDC, allow for authenticating and authorizing hosted and standalone apps with the same set of security characteristics.</span></span>

## <a name="authentication-process-with-oidc"></a><span data-ttu-id="ac265-128">Proceso de autenticación con OIDC</span><span class="sxs-lookup"><span data-stu-id="ac265-128">Authentication process with OIDC</span></span>

<span data-ttu-id="ac265-129">La biblioteca `Microsoft.AspNetCore.Components.WebAssembly.Authentication` ofrece varias primitivas para implementar la autenticación y autorización mediante OIDC.</span><span class="sxs-lookup"><span data-stu-id="ac265-129">The `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library offers several primitives to implement authentication and authorization using OIDC.</span></span> <span data-ttu-id="ac265-130">En términos generales, la autenticación funciona de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="ac265-130">In broad terms, authentication works as follows:</span></span>

* <span data-ttu-id="ac265-131">Cuando un usuario anónimo selecciona el botón de inicio de sesión o solicita una página con el atributo [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) aplicado, se le redirige a la página de inicio de sesión de la aplicación (`/authentication/login`).</span><span class="sxs-lookup"><span data-stu-id="ac265-131">When an anonymous user selects the login button or requests a page with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied, the user is redirected to the app's login page (`/authentication/login`).</span></span>
* <span data-ttu-id="ac265-132">En la página de inicio de sesión, la biblioteca de autenticación se prepara para un redireccionamiento al punto de conexión de autorización.</span><span class="sxs-lookup"><span data-stu-id="ac265-132">In the login page, the authentication library prepares for a redirect to the authorization endpoint.</span></span> <span data-ttu-id="ac265-133">El punto de conexión de autorización está fuera de la aplicación WebAssembly de Blazor y se puede hospedar en un origen independiente.</span><span class="sxs-lookup"><span data-stu-id="ac265-133">The authorization endpoint is outside of the Blazor WebAssembly app and can be hosted at a separate origin.</span></span> <span data-ttu-id="ac265-134">El punto de conexión es responsable de determinar si el usuario está autenticado y de emitir uno o más tokens en respuesta.</span><span class="sxs-lookup"><span data-stu-id="ac265-134">The endpoint is responsible for determining whether the user is authenticated and for issuing one or more tokens in response.</span></span> <span data-ttu-id="ac265-135">La biblioteca de autenticación proporciona una devolución de llamada de inicio de sesión para recibir la respuesta de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac265-135">The authentication library provides a login callback to receive the authentication response.</span></span>
  * <span data-ttu-id="ac265-136">Si el usuario no está autenticado, se le redirige al sistema de autenticación subyacente, que normalmente es ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac265-136">If the user isn't authenticated, the user is redirected to the underlying authentication system, which is usually ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="ac265-137">Si el usuario ya se ha autenticado, el punto de conexión de autorización genera los tokens adecuados y devuelve al explorador al punto de conexión de devolución de llamada de inicio de sesión (`/authentication/login-callback`).</span><span class="sxs-lookup"><span data-stu-id="ac265-137">If the user was already authenticated, the authorization endpoint generates the appropriate tokens and redirects the browser back to the login callback endpoint (`/authentication/login-callback`).</span></span>
* <span data-ttu-id="ac265-138">Cuando la aplicación WebAssembly de Blazor carga el punto de conexión de devolución de llamada de inicio de sesión (`/authentication/login-callback`), se procesa la respuesta de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac265-138">When the Blazor WebAssembly app loads the login callback endpoint (`/authentication/login-callback`), the authentication response is processed.</span></span>
  * <span data-ttu-id="ac265-139">Si el proceso de autenticación se completa correctamente, el usuario se autentica y, opcionalmente, se devuelve a la dirección URL protegida original que haya solicitado.</span><span class="sxs-lookup"><span data-stu-id="ac265-139">If the authentication process completes successfully, the user is authenticated and optionally sent back to the original protected URL that the user requested.</span></span>
  * <span data-ttu-id="ac265-140">Si por algún motivo se produce un error en el proceso de autenticación, se envía al usuario a la página de inicio de sesión con errores (`/authentication/login-failed`) y se muestra un error.</span><span class="sxs-lookup"><span data-stu-id="ac265-140">If the authentication process fails for any reason, the user is sent to the login failed page (`/authentication/login-failed`), and an error is displayed.</span></span>

## <a name="support-prerendering-with-authentication"></a><span data-ttu-id="ac265-141">Compatibilidad de la representación previa con la autenticación</span><span class="sxs-lookup"><span data-stu-id="ac265-141">Support prerendering with authentication</span></span>

<span data-ttu-id="ac265-142">Después de seguir las instrucciones que aparecen en uno de los temas de la aplicación WebAssembly de Blazor hospedada, use estas instrucciones para crear una aplicación que:</span><span class="sxs-lookup"><span data-stu-id="ac265-142">After following the guidance in one of the hosted Blazor WebAssembly app topics, use the following instructions to create an app that:</span></span>

* <span data-ttu-id="ac265-143">Representa previamente las rutas de acceso para las que no se requiere autorización.</span><span class="sxs-lookup"><span data-stu-id="ac265-143">Prerenders paths for which authorization isn't required.</span></span>
* <span data-ttu-id="ac265-144">No representa previamente las rutas de acceso para las que se requiere autorización.</span><span class="sxs-lookup"><span data-stu-id="ac265-144">Doesn't prerender paths for which authorization is required.</span></span>

<span data-ttu-id="ac265-145">En la clase `Program` de la aplicación cliente (*Program.cs*), factorice los registros de servicio comunes en un método independiente (por ejemplo, `ConfigureCommonServices`):</span><span class="sxs-lookup"><span data-stu-id="ac265-145">In the Client app's `Program` class (*Program.cs*), factor common service registrations into a separate method (for example, `ConfigureCommonServices`):</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.RootComponents.Add<App>("app");

        services.AddBaseAddressHttpClient();
        services.Add...;

        ConfigureCommonServices(builder.Services);

        await builder.Build().RunAsync();
    }

    public static void ConfigureCommonServices(IServiceCollection services)
    {
        // Common service registrations
    }
}
```

<span data-ttu-id="ac265-146">En `Startup.ConfigureServices` de la aplicación de servidor, registre estos servicios adicionales:</span><span class="sxs-lookup"><span data-stu-id="ac265-146">In the Server app's `Startup.ConfigureServices`, register the following additional services:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Server;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddRazorPages();
    services.AddScoped<AuthenticationStateProvider, ServerAuthenticationStateProvider>();
    services.AddScoped<SignOutSessionStateManager>();

    Client.Program.ConfigureCommonServices(services);
}
```

<span data-ttu-id="ac265-147">En el método `Startup.Configure` de la aplicación de servidor, reemplace `endpoints.MapFallbackToFile("index.html")` por `endpoints.MapFallbackToPage("/_Host")`:</span><span class="sxs-lookup"><span data-stu-id="ac265-147">In the Server app's `Startup.Configure` method, replace `endpoints.MapFallbackToFile("index.html")` with `endpoints.MapFallbackToPage("/_Host")`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

<span data-ttu-id="ac265-148">En la aplicación de servidor, cree una carpeta *Pages* si todavía no existe.</span><span class="sxs-lookup"><span data-stu-id="ac265-148">In the Server app, create a *Pages* folder if it doesn't exist.</span></span> <span data-ttu-id="ac265-149">Cree una página *_Host.cshtml* dentro de la carpeta *Pages* de la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="ac265-149">Create a *_Host.cshtml* page inside the Server app's *Pages* folder.</span></span> <span data-ttu-id="ac265-150">Pegue el contenido del archivo *wwwroot/index.html* de la aplicación cliente en el archivo *Pages/_Host.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ac265-150">Paste the contents from the Client app's *wwwroot/index.html* file into the *Pages/_Host.cshtml* file.</span></span> <span data-ttu-id="ac265-151">Actualice el contenido del archivo:</span><span class="sxs-lookup"><span data-stu-id="ac265-151">Update the file's contents:</span></span>

* <span data-ttu-id="ac265-152">Agregue `@page "_Host"` a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="ac265-152">Add `@page "_Host"` to the top of the file.</span></span>
* <span data-ttu-id="ac265-153">Reemplace la etiqueta `<app>Loading...</app>` por la siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac265-153">Replace the `<app>Loading...</app>` tag with the following:</span></span>

  ```cshtml
  <app>
      @if (!HttpContext.Request.Path.StartsWithSegments("/authentication"))
      {
          <component type="typeof(Wasm.Authentication.Client.App)" render-mode="Static" />
      }
      else
      {
          <text>Loading...</text>
      }
  </app>
  ```
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="ac265-154">Opciones para aplicaciones hospedadas y proveedores de inicio de sesión de terceros</span><span class="sxs-lookup"><span data-stu-id="ac265-154">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="ac265-155">Al autenticar y autorizar una aplicación WebAssembly de Blazor con un proveedor de terceros, hay varias opciones disponibles para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="ac265-155">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="ac265-156">Su elección depende del escenario.</span><span class="sxs-lookup"><span data-stu-id="ac265-156">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="ac265-157">Para obtener más información, vea <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="ac265-157">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="ac265-158">Autenticación de usuarios solo para llamadas a las API de terceros protegidas</span><span class="sxs-lookup"><span data-stu-id="ac265-158">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="ac265-159">Autentique al usuario con un flujo de OAuth del lado cliente en el proveedor de la API de terceros:</span><span class="sxs-lookup"><span data-stu-id="ac265-159">Authenticate the user with a client-side OAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="ac265-160">En este escenario:</span><span class="sxs-lookup"><span data-stu-id="ac265-160">In this scenario:</span></span>

* <span data-ttu-id="ac265-161">El servidor que hospeda la aplicación no desempeña ningún rol.</span><span class="sxs-lookup"><span data-stu-id="ac265-161">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="ac265-162">No se pueden proteger las API en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ac265-162">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="ac265-163">La aplicación solo puede llamar a las API de terceros protegidas.</span><span class="sxs-lookup"><span data-stu-id="ac265-163">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="ac265-164">Autenticación de usuarios con un proveedor de terceros y llamada a las API protegidas en el servidor host y en el tercero</span><span class="sxs-lookup"><span data-stu-id="ac265-164">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="ac265-165">Configure Identity con un proveedor de inicio de sesión de terceros.</span><span class="sxs-lookup"><span data-stu-id="ac265-165">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="ac265-166">Obtenga los tokens necesarios para el acceso de API de terceros y almacénelos.</span><span class="sxs-lookup"><span data-stu-id="ac265-166">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="ac265-167">Cuando un usuario inicia sesión, Identity recopila los tokens de acceso y actualización como parte del proceso de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac265-167">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="ac265-168">En ese momento, hay un par de enfoques disponibles para realizar llamadas API a las API de terceros.</span><span class="sxs-lookup"><span data-stu-id="ac265-168">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="ac265-169">Uso de un token de acceso de servidor para recuperar el token de acceso de terceros</span><span class="sxs-lookup"><span data-stu-id="ac265-169">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="ac265-170">Use el token de acceso generado en el servidor para recuperar el token de acceso de terceros de un punto de conexión de la API de servidor.</span><span class="sxs-lookup"><span data-stu-id="ac265-170">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="ac265-171">A partir de ahí, utilice el token de acceso de terceros para llamar directamente a los recursos de la API de terceros desde Identity en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ac265-171">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="ac265-172">Este enfoque no se recomienda.</span><span class="sxs-lookup"><span data-stu-id="ac265-172">We don't recommend this approach.</span></span> <span data-ttu-id="ac265-173">Este enfoque requiere tratar el token de acceso de terceros como si se hubiera generado para un cliente público.</span><span class="sxs-lookup"><span data-stu-id="ac265-173">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="ac265-174">En términos de OAuth, la aplicación pública no tiene un secreto de cliente porque no es de confianza para almacenar secretos de forma segura y el token de acceso se genera para un cliente confidencial.</span><span class="sxs-lookup"><span data-stu-id="ac265-174">In OAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="ac265-175">Un cliente confidencial es un cliente que tiene un secreto de cliente y se supone que puede almacenar secretos de forma segura.</span><span class="sxs-lookup"><span data-stu-id="ac265-175">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="ac265-176">El token de acceso de terceros podría recibir ámbitos adicionales para realizar operaciones confidenciales en función del hecho de que la tercera parte emitió el token para un cliente de más confianza.</span><span class="sxs-lookup"><span data-stu-id="ac265-176">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="ac265-177">Del mismo modo, los tokens de actualización no deben emitirse para un cliente que no sea de confianza, ya que esto proporciona acceso ilimitado al cliente a menos que se apliquen otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="ac265-177">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="ac265-178">Realización de llamadas API desde el cliente a la API de servidor para llamar a las API de terceros</span><span class="sxs-lookup"><span data-stu-id="ac265-178">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="ac265-179">Realice una llamada API desde el cliente a la API del servidor.</span><span class="sxs-lookup"><span data-stu-id="ac265-179">Make an API call from the client to the server API.</span></span> <span data-ttu-id="ac265-180">En el servidor, recupere el token de acceso para el recurso de la API de terceros y emita cualquier llamada que sea necesaria.</span><span class="sxs-lookup"><span data-stu-id="ac265-180">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="ac265-181">Aunque este enfoque requiere un salto de red adicional a través del servidor para llamar a una API de terceros, en última instancia resulta una experiencia más segura:</span><span class="sxs-lookup"><span data-stu-id="ac265-181">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="ac265-182">El servidor puede almacenar tokens de actualización y asegurarse de que la aplicación no pierde el acceso a recursos de terceros.</span><span class="sxs-lookup"><span data-stu-id="ac265-182">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="ac265-183">La aplicación no puede filtrar los tokens de acceso del servidor que puedan contener permisos más confidenciales.</span><span class="sxs-lookup"><span data-stu-id="ac265-183">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>
