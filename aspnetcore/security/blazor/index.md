---
title: Autenticación y autorización de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre los escenarios de autenticación y autorización de Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 04bbf20d1d848edfa98e719f316b790c812bfd95
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80501324"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a>Autenticación y autorización de ASP.NET Core Blazor

De [Steve Sanderson](https://github.com/SteveSandersonMS) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

> [!NOTE]
> Para ver las instrucciones que aparecen en este artículo y que se aplican a WebAssembly de Blazor, se requiere la versión 3.2 o posterior de la plantilla WebAssembly de Blazor de ASP.NET Core. Si no usa Visual Studio 16.6 Preview 2 o posterior, obtenga la plantilla WebAssembly de Blazor más reciente si sigue las instrucciones que aparecen en <xref:blazor/get-started>.

ASP.NET Core admite la configuración y administración de seguridad en las aplicaciones de Blazor.

Los escenarios de seguridad varían según si las aplicaciones son del servidor de Blazor o Blazor WebAssembly. Debido a que las aplicaciones del servidor de Blazor se ejecutan en el servidor, las comprobaciones de autorización pueden determinar:

* Las opciones de la interfaz de usuario presentadas a un usuario (por ejemplo, qué entradas de menú están disponibles para el usuario).
* Las reglas de acceso para las áreas de la aplicación y los componentes.

Las aplicaciones de Blazor WebAssembly se ejecutan en el cliente. La autorización *solo* se utiliza para determinar qué opciones de la interfaz de usuario se van a mostrar. Dado que el usuario puede modificar u omitir las comprobaciones en el cliente, las aplicaciones de Blazor WebAssembly no pueden aplicar reglas de acceso de autorización.

Las [convenciones de autorización de Razor Pages](xref:security/authorization/razor-pages-authorization) no se aplican a los componentes Razor enrutables. Si un componente Razor no enrutable se [inserta en una página](xref:blazor/integrate-components#render-components-from-a-page-or-view), las convenciones de autorización de la página afectan indirectamente al componente Razor y al resto del contenido de la página.

> [!NOTE]
> <xref:Microsoft.AspNetCore.Identity.SignInManager%601> y <xref:Microsoft.AspNetCore.Identity.UserManager%601> no se admiten en los componentes de Razor.

## <a name="authentication"></a>Autenticación

Blazor usa los mecanismos de autenticación de ASP.NET Core existentes para establecer la identidad del usuario. El mecanismo exacto depende de cómo se hospeda la aplicación Blazor, WebAssembly de Blazor o Blazor Server.

### <a name="opno-locblazor-webassembly-authentication"></a>Autenticación de Blazor WebAssembly

En las aplicaciones de Blazor WebAssembly, las comprobaciones de autenticación pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente. Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.

Agregue la siguiente línea de código:

* Una referencia de paquete para [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) al archivo de proyecto de la aplicación.
* El espacio de nombres `Microsoft.AspNetCore.Components.Authorization` al archivo *_Imports.razor* de la aplicación.

Para controlar la autenticación, la implementación de un servicio `AuthenticationStateProvider` integrado o personalizado se describe en las secciones siguientes.

Para más información sobre cómo crear aplicaciones y su configuración, consulte <xref:security/blazor/webassembly/index>.

### <a name="opno-locblazor-server-authentication"></a>Autenticación del servidor de Blazor

Las aplicaciones del servidor de Blazor funcionan mediante una conexión en tiempo real que se crea con SignalR. La [autenticación en aplicaciones basadas en SignalR](xref:signalr/authn-and-authz) se controla cuando se establece la conexión. La autenticación se puede basar en una cookie o en cualquier otro token de portador.

Para más información sobre cómo crear aplicaciones y su configuración, consulte <xref:security/blazor/server>.

## <a name="authenticationstateprovider-service"></a>Servicio AuthenticationStateProvider

El servicio `AuthenticationStateProvider` integrado obtiene datos de estado de autenticación de `HttpContext.User` de ASP.NET Core. Así es como el estado de autenticación se integra con los mecanismos de autenticación de ASP.NET Core.

`AuthenticationStateProvider` es el servicio subyacente utilizado por el componente `AuthorizeView` y el componente `CascadingAuthenticationState` para obtener el estado de autenticación.

Por lo general, no se utiliza `AuthenticationStateProvider` directamente. Use los enfoques del [componente AuthorizeView](#authorizeview-component) o [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) que se describen más adelante en este artículo. El principal inconveniente de utilizar `AuthenticationStateProvider` directamente es que el componente no se notifica de manera automática si cambia el estado de autenticación subyacente de los datos.

El servicio `AuthenticationStateProvider` puede proporcionar los datos <xref:System.Security.Claims.ClaimsPrincipal> del usuario actual, como se muestra en el ejemplo siguiente:

```razor
@page "/"
@using System.Security.Claims
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<h3>ClaimsPrincipal Data</h3>

<button @onclick="GetClaimsPrincipalData">Get ClaimsPrincipal Data</button>

<p>@_authMessage</p>

@if (_claims.Count() > 0)
{
    <ul>
        @foreach (var claim in _claims)
        {
            <li>@claim.Type &ndash; @claim.Value</li>
        }
    </ul>
}

<p>@_surnameMessage</p>

@code {
    private string _authMessage;
    private string _surnameMessage;
    private IEnumerable<Claim> _claims = Enumerable.Empty<Claim>();

    private async Task GetClaimsPrincipalData()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
            _claims = user.Claims;
            _surnameMessage = 
                $"Surname: {user.FindFirst(c => c.Type == ClaimTypes.Surname)?.Value}";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

Si `user.Identity.IsAuthenticated` es `true` y porque el usuario es <xref:System.Security.Claims.ClaimsPrincipal>, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.

Para más información sobre la inserción de dependencias (DI) y servicios, consulte <xref:blazor/dependency-injection> y <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementación de un componente AuthenticationStateProvider personalizado

Si la aplicación requiere un proveedor personalizado, implemente `AuthenticationStateProvider` e invalide `GetAuthenticationStateAsync`:

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

public class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

En una aplicación WebAssembly de Blazor, el servicio `CustomAuthStateProvider` se registra en `Main` de *Program.cs*:

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

En una aplicación de Blazor Server, el servicio `CustomAuthStateProvider` está registrado en `Startup.ConfigureServices`:

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

Con el `CustomAuthStateProvider` en el ejemplo anterior, todos los usuarios se autentican con el nombre de usuario `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Exposición del estado de autenticación como un parámetro en cascada

Si se requieren los datos de estado de autenticación para la lógica de procedimiento, como cuando se realiza una acción desencadenada por el usuario, obtenga los datos de estado de autenticación mediante la definición de un parámetro en cascada del tipo `Task<AuthenticationState>`:

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

<p>@_authMessage</p>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private string _authMessage;

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

Si `user.Identity.IsAuthenticated` es `true`, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.

Configure el parámetro en cascada`Task<AuthenticationState>` mediante los componentes `AuthorizeRouteView` y `CascadingAuthenticationState` en el componente `App` (*App.razor*):

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

En una aplicación WebAssembly de Blazor, agregue servicios para opciones y autorización a `Program.Main`:

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

En una aplicación de Blazor Server, ya existen los servicios para opciones y autorización, por lo que no se requiere realizar ninguna otra acción.

## <a name="authorization"></a>Autorización

Cuando un usuario está autenticado, se aplican las reglas de *autorización* para controlar qué puede hacer el usuario.

Por lo general, se concede o deniega el acceso en función de si:

* El usuario está autenticado (ha iniciado sesión).
* El usuario está en un *rol*.
* El usuario tiene una *notificación*.
* Una *directiva* se cumple.

Cada uno de estos conceptos es el mismo que en una aplicación ASP.NET Core MVC o Razor Pages. Para más información sobre la seguridad de ASP.NET Core, consulte los artículos que se encuentran en [ASP.NET Core Security and Identity](xref:security/index) (Identidad y seguridad de ASP.NET Core).

## <a name="authorizeview-component"></a>Componente AuthorizeView

El componente `AuthorizeView` selectivamente muestra la interfaz de usuario dependiendo de si el usuario está autorizado para verlo. Este enfoque es útil cuando solo necesite *mostrar* datos del usuario y no es necesario usar la identidad del usuario en la lógica de procedimientos.

El componente expone una variable `context` del tipo `AuthenticationState`, que puede utilizar para acceder a la información sobre el usuario que ha iniciado sesión:

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

También puede proporcionar contenido diferente para mostrar si el usuario no está autenticado:

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

El componente `AuthorizeView` se puede usar en el componente `NavMenu` (*Shared/NavMenu.razor*) para mostrar un elemento de lista (`<li>...</li>`) para un `NavLink`, pero tenga en cuenta que este enfoque solo quita el elemento de lista desde la salida representada. No impide que el usuario navegue hasta el componente.

El contenido de las etiquetas `<Authorized>` y `<NotAuthorized>` puede incluir elementos arbitrarios, como otros componentes interactivos.

Las condiciones de autorización, como los roles o directivas que controlan las opciones o el acceso a la interfaz de usuario, se tratan en la sección [Autorización](#authorization).

Si no se especifican las condiciones de la autorización, `AuthorizeView` usa una directiva predeterminada y trata:

* A los usuarios autenticados (con sesión iniciada) como autorizados.
* A los usuarios no autenticados (sin sesión no iniciada) como no autorizados.

### <a name="role-based-and-policy-based-authorization"></a>Autorización basada en roles y en directivas

El componente `AuthorizeView` admite la autorización *basada en roles* o *basada en directivas*.

Para la autorización basada en roles, utilice el parámetro `Roles`:

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Para obtener más información, vea <xref:security/authorization/roles>.

Para la autorización basada en directivas, utilice el parámetro `Policy`:

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

La autorización basada en notificaciones es un caso especial de autorización basada en directivas. Por ejemplo, puede definir una directiva que requiere que los usuarios tengan una notificación determinada. Para obtener más información, vea <xref:security/authorization/policies>.

Estas API se pueden usar en las aplicaciones del servidor de Blazor o Blazor WebAssembly.

Si no se especifica `Roles` ni `Policy`, `AuthorizeView` usa la directiva predeterminada.

### <a name="content-displayed-during-asynchronous-authentication"></a>Contenido que se muestra durante la autenticación asincrónica

Blazor permite que el estado de autenticación se determine *asincrónicamente*. El escenario principal de este enfoque se encuentra en las aplicaciones de Blazor WebAssembly que realizan una solicitud de autenticación a un punto de conexión externo.

Mientras la autenticación está en curso, `AuthorizeView` no muestra ningún contenido de forma predeterminada. Para mostrar el contenido mientras tiene lugar la autenticación, use el elemento `<Authorizing>`:

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

Este enfoque no se aplica normalmente a las aplicaciones del servidor de Blazor. Las aplicaciones del servidor de Blazor conocen el estado de autenticación tan pronto como se establece dicho estado. El contenido `Authorizing` puede proporcionarse en el componente `AuthorizeView` de una aplicación del servidor de Blazor, pero nunca se muestra.

## <a name="authorize-attribute"></a>Atributo [Authorize]

El atributo `[Authorize]` se puede usar en los componentes de Razor:

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Utilice únicamente `[Authorize]` en componentes `@page` a los que se llega a través del enrutador de Blazor. La autorización solo se realiza como un aspecto del enrutamiento y *no* para los componentes secundarios representados dentro de una página. Para autorizar la presentación de partes concretas dentro de una página, use `AuthorizeView` en su lugar.

El atributo `[Authorize]` admite también la autorización basada en roles o basada en directivas. Para la autorización basada en roles, utilice el parámetro `Roles`:

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Para la autorización basada en directivas, utilice el parámetro `Policy`:

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Si no se especifica `Roles` ni `Policy`, `[Authorize]` usa la directiva predeterminada, que consiste en tratar:

* A los usuarios autenticados (con sesión iniciada) como autorizados.
* A los usuarios no autenticados (sin sesión no iniciada) como no autorizados.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Personalización del contenido no autorizado con el componente de enrutador

El componente `Router`, junto con el componente `AuthorizeRouteView`, permite que la aplicación especifique el contenido personalizado si:

* No se encuentra el contenido.
* El usuario produce un error en la condición `[Authorize]` aplicada al componente. El atributo `[Authorize]` se describe en la sección [Atributo `[Authorize]`](#authorize-attribute).
* La autenticación asincrónica está en curso.

En la plantilla de proyecto de Blazor Server predeterminada, el componente `App` (*App.razor*) muestra cómo configurar el contenido personalizado:

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

El contenido de las etiquetas `<NotFound>`, `<NotAuthorized>` y `<Authorizing>` puede incluir elementos arbitrarios, como otros componentes interactivos.

Si el elemento `<NotAuthorized>` no se especifica, `AuthorizeRouteView` utiliza el siguiente mensaje de reserva:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Notificación sobre los cambios de estado de autenticación

Si la aplicación determina que los datos de estado de autenticación subyacentes han cambiado (por ejemplo, porque el usuario ha cerrado sesión o porque otro usuario ha cambiado sus roles), un [AuthenticationStateProvider personalizado](#implement-a-custom-authenticationstateprovider) puede opcionalmente invocar el método `NotifyAuthenticationStateChanged` en la clase base `AuthenticationStateProvider`. Esto notifica a los consumidores de los datos de estado de autenticación (por ejemplo, `AuthorizeView`) para que los vuelvan a procesar utilizando los nuevos datos.

## <a name="procedural-logic"></a>Lógica de procedimientos

Si se requiere que la aplicación compruebe las reglas de autorización como parte de la lógica de procedimiento, utilice un parámetro en cascada del tipo `Task<AuthenticationState>` para obtener el <xref:System.Security.Claims.ClaimsPrincipal> del usuario. `Task<AuthenticationState>` se puede combinar con otros servicios, como `IAuthorizationService`, para evaluar las directivas.

```razor
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

> [!NOTE]
> En un componente de la aplicación de Blazor WebAssembly, agregue los espacios de nombres `Microsoft.AspNetCore.Authorization` y `Microsoft.AspNetCore.Components.Authorization`:
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> Estos espacios de nombres se pueden proporcionar globalmente si se agregan al archivo *_Imports.razor* de la aplicación.

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Autorización en aplicaciones de Blazor WebAssembly

En aplicaciones de Blazor WebAssembly, las comprobaciones de autorización pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente. Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.

**Realice siempre las comprobaciones de autorización en el servidor dentro de cualquier punto de conexión de la API al que acceda su aplicación del lado cliente.**

Para obtener más información, vea los artículos en <xref:security/blazor/webassembly/index>.

## <a name="troubleshoot-errors"></a>Solucionar problemas

Errores comunes:

* **La autorización requiere un parámetro en casada de tipo Task\<AuthenticationState>. Considere el uso de CascadingAuthenticationState para proporcionarla.**

* **Se recibe el valor `null` para `authenticationStateTask`**

Es probable que el proyecto no se haya creado mediante una plantilla del servidor de Blazor con la autenticación habilitada. Encapsule un `<CascadingAuthenticationState>` alrededor de alguna parte del árbol de la interfaz de usuario, por ejemplo, en el componente `App` (*App.razor*) de la siguiente manera:

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

El `CascadingAuthenticationState` proporciona el parámetro en cascada `Task<AuthenticationState>`, que a su vez recibe el servicio DI `AuthenticationStateProvider` subyacente.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* [GenialBlazor: Vínculos de ejemplo de la comunidad de autenticación](https://github.com/AdrienTorris/awesome-blazor#authentication)
