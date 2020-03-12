---
title: ASP.NET Core Blazor escenarios de seguridad adicionales de webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: fe87ce76d8e181de788188b021616f2a09833585
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083696"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>ASP.NET Core más escenarios de seguridad adicionales de webassembly

Por [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>Controlar errores de solicitudes de token

Cuando una aplicación de una sola página (SPA) autentica a un usuario mediante Open ID Connect (OIDC), el estado de autenticación se mantiene localmente en el SPA y en el proveedor de identidades (IP) en forma de una cookie de sesión que se establece como resultado del usuario que proporciona sus credenciales.

Normalmente, los tokens que emite la IP para el usuario son válidos durante breves períodos de tiempo, aproximadamente una hora, por lo que la aplicación cliente debe capturar nuevos tokens con regularidad. De lo contrario, se cerrará la sesión del usuario después de que expiren los tokens concedidos. En la mayoría de los casos, los clientes de OIDC pueden aprovisionar nuevos tokens sin necesidad de que el usuario se autentique de nuevo gracias al estado de autenticación o a la "sesión" que se mantiene dentro de la dirección IP.

Hay algunos casos en los que el cliente no puede obtener un token sin la interacción del usuario, por ejemplo, cuando por alguna razón el usuario cierra la sesión explícitamente desde la dirección IP. Este escenario se produce si un usuario visita `https://login.microsoftonline.com` y cierra la sesión. En estos casos, la aplicación no sabe inmediatamente que el usuario ha cerrado la sesión. Es posible que el token que contiene el cliente ya no sea válido. Además, el cliente no puede aprovisionar un token nuevo sin la interacción del usuario después de que expire el token actual.

Estos escenarios no son específicos de la autenticación basada en tokens. Forman parte de la naturaleza de spa. Un SPA que use cookies tampoco podrá llamar a una API de servidor si se quita la cookie de autenticación.

Cuando una aplicación realiza llamadas API a recursos protegidos, debe tener en cuenta lo siguiente:

* Para aprovisionar un nuevo token de acceso para llamar a la API, es posible que sea necesario volver a autenticar al usuario.
* Incluso si el cliente tiene un token que parece ser válido, es posible que se produzca un error en la llamada al servidor porque el usuario revocó el token.

Cuando la aplicación solicita un token, hay dos resultados posibles:

* La solicitud se realiza correctamente y la aplicación tiene un token válido.
* Se produce un error en la solicitud y la aplicación debe autenticar al usuario de nuevo para obtener un nuevo token.

Cuando se produce un error en una solicitud de token, debe decidir si desea guardar el estado actual antes de realizar una redirección. Existen varios enfoques con niveles crecientes de complejidad:

* Almacenar el estado de la página actual en el almacenamiento de sesión. Durante `OnInitializeAsync`, compruebe si el estado se puede restaurar antes de continuar.
* Agregue un parámetro de cadena de consulta y úselo como una manera de señalar la aplicación que necesita para volver a hidratar el estado guardado anteriormente.
* Agregue un parámetro de cadena de consulta con un identificador único para almacenar los datos en el almacenamiento de sesión sin poner en peligro las colisiones con otros elementos.

El ejemplo siguiente muestra cómo:

* Conservar el estado antes de redirigir a la página de inicio de sesión.
* Recupere el estado anterior después de la autenticación mediante el parámetro de cadena de consulta.

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

## <a name="save-app-state-before-an-authentication-operation"></a>Guardar el estado de la aplicación antes de una operación de autenticación

Durante una operación de autenticación, hay casos en los que desea guardar el estado de la aplicación antes de que el explorador se redirija a la dirección IP. Esto puede ser así cuando se usa algo como un contenedor de estado y se desea restaurar el estado después de que la autenticación se realice correctamente. Puede usar un objeto de estado de autenticación personalizado para conservar el estado específico de la aplicación o una referencia a él y restaurar ese estado una vez que la operación de autenticación se complete correctamente.

componente de `Authentication` (*páginas/autenticación. Razor*):

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

## <a name="request-additional-access-tokens"></a>Solicitar tokens de acceso adicionales

La mayoría de las aplicaciones solo requieren un token de acceso para interactuar con los recursos protegidos que usan. En algunos escenarios, una aplicación podría requerir más de un token para interactuar con dos o más recursos. El método `IAccessTokenProvider.RequestToken` proporciona una sobrecarga que permite a una aplicación aprovisionar un token con un conjunto determinado de ámbitos, tal como se muestra en el ejemplo siguiente:

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>Personalización de rutas de aplicación

De forma predeterminada, la biblioteca de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` usa las rutas que se muestran en la tabla siguiente para representar diferentes Estados de autenticación.

| Enrutar                            | Propósito |
| -------------------------------- | ------- |
| `authentication/login`           | Desencadena una operación de inicio de sesión. |
| `authentication/login-callback`  | Controla el resultado de cualquier operación de inicio de sesión. |
| `authentication/login-failed`    | Muestra mensajes de error cuando se produce un error en la operación de inicio de sesión por algún motivo. |
| `authentication/logout`          | Desencadena una operación de cierre de sesión. |
| `authentication/logout-callback` | Controla el resultado de una operación de cierre de sesión. |
| `authentication/logout-failed`   | Muestra mensajes de error cuando se produce un error en la operación de cierre de sesión por algún motivo. |
| `authentication/logged-out`      | Indica que el usuario ha cerrado la sesión correctamente. |
| `authentication/profile`         | Desencadena una operación para editar el perfil de usuario. |
| `authentication/register`        | Desencadena una operación para registrar un nuevo usuario. |

Las rutas que se muestran en la tabla anterior se pueden configurar a través de `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`. Al establecer las opciones para proporcionar rutas personalizadas, confirme que la aplicación tiene una ruta que controla cada ruta de acceso.

En el ejemplo siguiente, todas las rutas de acceso tienen el prefijo `/security`.

componente de `Authentication` (*páginas/autenticación. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` (*Program.CS*):

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

Si el requisito requiere rutas de acceso completamente diferentes, establezca las rutas como se describió anteriormente y represente el `RemoteAuthenticatorView` con un parámetro de acción explícito:

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

Puede descomponer la interfaz de usuario en diferentes páginas si decide hacerlo.

## <a name="customize-the-authentication-user-interface"></a>Personalización de la interfaz de usuario de autenticación

`RemoteAuthenticatorView` incluye un conjunto predeterminado de partes de la interfaz de usuario para cada estado de autenticación. Cada Estado se puede personalizar pasando un `RenderFragment`personalizado. Para personalizar el texto mostrado durante el proceso de inicio de sesión inicial, puede cambiar el `RemoteAuthenticatorView` como se indica a continuación.

componente de `Authentication` (*páginas/autenticación. Razor*):

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

El `RemoteAuthenticatorView` tiene un fragmento que se puede utilizar por cada ruta de autenticación que se muestra en la tabla siguiente.

| Enrutar                            | Fragmento             |
| -------------------------------- | -------------------- |
| `authentication/login`           | `<LoggingIn>`        |
| `authentication/login-callback`  | `<CompletingLogIn>`  |
| `authentication/login-failed`    | `<LogInFailed>`      |
| `authentication/logout`          | `<LoggingOut>`       |
| `authentication/logout-callback` | `<CompletingLogOut>` |
| `authentication/logout-failed`   | `<LogOutFailed>`     |
| `authentication/logged-out`      | `<LogOutSucceeded>`  |
| `authentication/profile`         | `<UserProfile>`      |
| `authentication/register`        | `<Registering>`      |
