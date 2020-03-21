---
title: ASP.NET Core Blazor escenarios de seguridad adicionales de webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989457"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="ac37e-102">ASP.NET Core más escenarios de seguridad adicionales de webassembly</span><span class="sxs-lookup"><span data-stu-id="ac37e-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="ac37e-103">Por [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="ac37e-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a><span data-ttu-id="ac37e-104">Controlar errores de solicitudes de token</span><span class="sxs-lookup"><span data-stu-id="ac37e-104">Handle token request errors</span></span>

<span data-ttu-id="ac37e-105">Cuando una aplicación de una sola página (SPA) autentica a un usuario mediante Open ID Connect (OIDC), el estado de autenticación se mantiene localmente en el SPA y en el proveedor de identidades (IP) en forma de una cookie de sesión que se establece como resultado del usuario que proporciona su sus.</span><span class="sxs-lookup"><span data-stu-id="ac37e-105">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="ac37e-106">Normalmente, los tokens que emite la IP para el usuario son válidos durante breves períodos de tiempo, aproximadamente una hora, por lo que la aplicación cliente debe capturar nuevos tokens con regularidad.</span><span class="sxs-lookup"><span data-stu-id="ac37e-106">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="ac37e-107">De lo contrario, se cerrará la sesión del usuario después de que expiren los tokens concedidos.</span><span class="sxs-lookup"><span data-stu-id="ac37e-107">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="ac37e-108">En la mayoría de los casos, los clientes de OIDC pueden aprovisionar nuevos tokens sin necesidad de que el usuario se autentique de nuevo gracias al estado de autenticación o a la "sesión" que se mantiene dentro de la dirección IP.</span><span class="sxs-lookup"><span data-stu-id="ac37e-108">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="ac37e-109">Hay algunos casos en los que el cliente no puede obtener un token sin la interacción del usuario, por ejemplo, cuando por alguna razón el usuario cierra la sesión explícitamente desde la dirección IP.</span><span class="sxs-lookup"><span data-stu-id="ac37e-109">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="ac37e-110">Este escenario se produce si un usuario visita `https://login.microsoftonline.com` y cierra la sesión. En estos casos, la aplicación no sabe inmediatamente que el usuario ha cerrado la sesión. Es posible que el token que contiene el cliente ya no sea válido.</span><span class="sxs-lookup"><span data-stu-id="ac37e-110">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="ac37e-111">Además, el cliente no puede aprovisionar un token nuevo sin la interacción del usuario después de que expire el token actual.</span><span class="sxs-lookup"><span data-stu-id="ac37e-111">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="ac37e-112">Estos escenarios no son específicos de la autenticación basada en tokens.</span><span class="sxs-lookup"><span data-stu-id="ac37e-112">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="ac37e-113">Forman parte de la naturaleza de spa.</span><span class="sxs-lookup"><span data-stu-id="ac37e-113">They are part of the nature of SPAs.</span></span> <span data-ttu-id="ac37e-114">Un SPA que use cookies tampoco podrá llamar a una API de servidor si se quita la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac37e-114">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="ac37e-115">Cuando una aplicación realiza llamadas API a recursos protegidos, debe tener en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac37e-115">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="ac37e-116">Para aprovisionar un nuevo token de acceso para llamar a la API, es posible que sea necesario volver a autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="ac37e-116">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="ac37e-117">Incluso si el cliente tiene un token que parece ser válido, es posible que se produzca un error en la llamada al servidor porque el usuario revocó el token.</span><span class="sxs-lookup"><span data-stu-id="ac37e-117">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="ac37e-118">Cuando la aplicación solicita un token, hay dos resultados posibles:</span><span class="sxs-lookup"><span data-stu-id="ac37e-118">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="ac37e-119">La solicitud se realiza correctamente y la aplicación tiene un token válido.</span><span class="sxs-lookup"><span data-stu-id="ac37e-119">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="ac37e-120">Se produce un error en la solicitud y la aplicación debe autenticar al usuario de nuevo para obtener un nuevo token.</span><span class="sxs-lookup"><span data-stu-id="ac37e-120">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="ac37e-121">Cuando se produce un error en una solicitud de token, debe decidir si desea guardar el estado actual antes de realizar una redirección.</span><span class="sxs-lookup"><span data-stu-id="ac37e-121">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="ac37e-122">Existen varios enfoques con niveles crecientes de complejidad:</span><span class="sxs-lookup"><span data-stu-id="ac37e-122">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="ac37e-123">Almacenar el estado de la página actual en el almacenamiento de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-123">Store the current page state in session storage.</span></span> <span data-ttu-id="ac37e-124">Durante `OnInitializeAsync`, compruebe si el estado se puede restaurar antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ac37e-124">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="ac37e-125">Agregue un parámetro de cadena de consulta y úselo como una manera de señalar la aplicación que necesita para volver a hidratar el estado guardado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ac37e-125">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="ac37e-126">Agregue un parámetro de cadena de consulta con un identificador único para almacenar los datos en el almacenamiento de sesión sin poner en peligro las colisiones con otros elementos.</span><span class="sxs-lookup"><span data-stu-id="ac37e-126">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="ac37e-127">El ejemplo siguiente muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="ac37e-127">The following example shows how to:</span></span>

* <span data-ttu-id="ac37e-128">Conservar el estado antes de redirigir a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-128">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="ac37e-129">Recupere el estado anterior después de la autenticación mediante el parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="ac37e-129">Recover the previous state afterward authentication using the query string parameter.</span></span>

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="ac37e-130">Guardar el estado de la aplicación antes de una operación de autenticación</span><span class="sxs-lookup"><span data-stu-id="ac37e-130">Save app state before an authentication operation</span></span>

<span data-ttu-id="ac37e-131">Durante una operación de autenticación, hay casos en los que desea guardar el estado de la aplicación antes de que el explorador se redirija a la dirección IP.</span><span class="sxs-lookup"><span data-stu-id="ac37e-131">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="ac37e-132">Esto puede ser así cuando se usa algo como un contenedor de estado y se desea restaurar el estado después de que la autenticación se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="ac37e-132">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="ac37e-133">Puede usar un objeto de estado de autenticación personalizado para conservar el estado específico de la aplicación o una referencia a él y restaurar ese estado una vez que la operación de autenticación se complete correctamente.</span><span class="sxs-lookup"><span data-stu-id="ac37e-133">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="ac37e-134">componente de `Authentication` (*páginas/autenticación. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ac37e-134">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a><span data-ttu-id="ac37e-135">Solicitar tokens de acceso adicionales</span><span class="sxs-lookup"><span data-stu-id="ac37e-135">Request additional access tokens</span></span>

<span data-ttu-id="ac37e-136">La mayoría de las aplicaciones solo requieren un token de acceso para interactuar con los recursos protegidos que usan.</span><span class="sxs-lookup"><span data-stu-id="ac37e-136">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="ac37e-137">En algunos escenarios, una aplicación podría requerir más de un token para interactuar con dos o más recursos.</span><span class="sxs-lookup"><span data-stu-id="ac37e-137">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span> <span data-ttu-id="ac37e-138">El método `IAccessTokenProvider.RequestToken` proporciona una sobrecarga que permite a una aplicación aprovisionar un token con un conjunto determinado de ámbitos, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac37e-138">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a><span data-ttu-id="ac37e-139">Personalización de rutas de aplicación</span><span class="sxs-lookup"><span data-stu-id="ac37e-139">Customize app routes</span></span>

<span data-ttu-id="ac37e-140">De forma predeterminada, la biblioteca de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` usa las rutas que se muestran en la tabla siguiente para representar diferentes Estados de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac37e-140">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="ac37e-141">Enrutar</span><span class="sxs-lookup"><span data-stu-id="ac37e-141">Route</span></span>                            | <span data-ttu-id="ac37e-142">Propósito</span><span class="sxs-lookup"><span data-stu-id="ac37e-142">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="ac37e-143">Desencadena una operación de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-143">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="ac37e-144">Controla el resultado de cualquier operación de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-144">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="ac37e-145">Muestra mensajes de error cuando se produce un error en la operación de inicio de sesión por algún motivo.</span><span class="sxs-lookup"><span data-stu-id="ac37e-145">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="ac37e-146">Desencadena una operación de cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-146">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="ac37e-147">Controla el resultado de una operación de cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="ac37e-147">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="ac37e-148">Muestra mensajes de error cuando se produce un error en la operación de cierre de sesión por algún motivo.</span><span class="sxs-lookup"><span data-stu-id="ac37e-148">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="ac37e-149">Indica que el usuario ha cerrado la sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="ac37e-149">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="ac37e-150">Desencadena una operación para editar el perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="ac37e-150">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="ac37e-151">Desencadena una operación para registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="ac37e-151">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="ac37e-152">Las rutas que se muestran en la tabla anterior se pueden configurar a través de `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span><span class="sxs-lookup"><span data-stu-id="ac37e-152">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="ac37e-153">Al establecer las opciones para proporcionar rutas personalizadas, confirme que la aplicación tiene una ruta que controla cada ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="ac37e-153">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="ac37e-154">En el ejemplo siguiente, todas las rutas de acceso tienen el prefijo `/security`.</span><span class="sxs-lookup"><span data-stu-id="ac37e-154">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="ac37e-155">componente de `Authentication` (*páginas/autenticación. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ac37e-155">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="ac37e-156">`Program.Main` (*Program.CS*):</span><span class="sxs-lookup"><span data-stu-id="ac37e-156">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

<span data-ttu-id="ac37e-157">Si el requisito requiere rutas de acceso completamente diferentes, establezca las rutas como se describió anteriormente y represente el `RemoteAuthenticatorView` con un parámetro de acción explícito:</span><span class="sxs-lookup"><span data-stu-id="ac37e-157">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="ac37e-158">Puede descomponer la interfaz de usuario en diferentes páginas si decide hacerlo.</span><span class="sxs-lookup"><span data-stu-id="ac37e-158">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="ac37e-159">Personalización de la interfaz de usuario de autenticación</span><span class="sxs-lookup"><span data-stu-id="ac37e-159">Customize the authentication user interface</span></span>

<span data-ttu-id="ac37e-160">`RemoteAuthenticatorView` incluye un conjunto predeterminado de partes de la interfaz de usuario para cada estado de autenticación.</span><span class="sxs-lookup"><span data-stu-id="ac37e-160">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="ac37e-161">Cada Estado se puede personalizar pasando un `RenderFragment`personalizado.</span><span class="sxs-lookup"><span data-stu-id="ac37e-161">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="ac37e-162">Para personalizar el texto mostrado durante el proceso de inicio de sesión inicial, puede cambiar el `RemoteAuthenticatorView` como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="ac37e-162">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="ac37e-163">componente de `Authentication` (*páginas/autenticación. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ac37e-163">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="ac37e-164">El `RemoteAuthenticatorView` tiene un fragmento que se puede utilizar por cada ruta de autenticación que se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac37e-164">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="ac37e-165">Enrutar</span><span class="sxs-lookup"><span data-stu-id="ac37e-165">Route</span></span>                            | <span data-ttu-id="ac37e-166">Fragmento</span><span class="sxs-lookup"><span data-stu-id="ac37e-166">Fragment</span></span>                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |
