---
title: Autenticación y autorización de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre los escenarios de autenticación y autorización de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: ab8cc547463ef647316b5a4e377c15021debc4b1
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963964"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>Autenticación y autorización de ASP.NET Core Blazor

Por [Steve Sanderson](https://github.com/SteveSandersonMS)

ASP.NET Core admite la configuración y administración de seguridad en las aplicaciones Blazor.

Los escenarios de seguridad varían según si las aplicaciones son de servidor Blazor o WebAssembly de Blazor. Debido a que las aplicaciones de servidor Blazor se ejecutan en el servidor, las comprobaciones de autorización pueden determinar:

* Las opciones de la interfaz de usuario presentadas a un usuario (por ejemplo, qué entradas de menú están disponibles para el usuario).
* Las reglas de acceso para las áreas de la aplicación y los componentes.

Las aplicaciones WebAssembly de Blazor se ejecutan en el cliente. La autorización *solo* se utiliza para determinar qué opciones de la interfaz de usuario se van a mostrar. Dado que el usuario puede modificar u omitir las comprobaciones en el cliente, las aplicaciones WebAssembly de Blazor no puede aplicar reglas de acceso de autorización.

## <a name="authentication"></a>Autenticación

Blazor utiliza los mecanismos de autenticación de ASP.NET Core existentes para establecer la identidad del usuario. El mecanismo exacto depende de la forma en que se hospeda la aplicación: servidor Blazor o WebAssembly de Blazor.

### <a name="blazor-server-authentication"></a>Autenticación del servidor Blazor

Las aplicaciones de servidor Blazor funcionan mediante una conexión en tiempo real que se crea con SignalR. [La autenticación en aplicaciones basadas en SignalR](xref:signalr/authn-and-authz) se controla cuando se establece la conexión. La autenticación se puede basar en una cookie o en cualquier otro token de portador.

La plantilla de proyecto de servidor Blazor puede configurar la autenticación cuando se crea el proyecto.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Siga las instrucciones de Visual Studio que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación.

Después de elegir la plantilla **Aplicación de servidor Blazor** en el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **Cambiar** en **Autenticación**.

Se abre un cuadro de diálogo para ofrecer el mismo conjunto de mecanismos de autenticación disponibles para otros proyectos ASP.NET Core:

* **Sin autenticación**
* **Cuentas de usuario individuales**. Las cuentas de usuario se pueden almacenar:
  * Dentro de la aplicación mediante el sistema de [identidad](xref:security/authentication/identity) de ASP.NET Core.
  * Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c)
* **Cuentas profesionales o educativas**
* **Autenticación de Windows**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Siga las instrucciones de Visual Studio Code que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).

| Mecanismo de autenticación                                                                 | Valor de`{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Sin autenticación                                                                        | `None`                   |
| Individual<br>Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.                        | `Individual`             |
| Individual<br>Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Cuentas profesionales o educativas<br>Autenticación organizativa para un solo inquilino.            | `SingleOrg`              |
| Cuentas profesionales o educativas<br>Autenticación organizativa para varios inquilinos.           | `MultiOrg`               |
| Autenticación de Windows                                                                   | `Windows`                |

El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación. Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-webassembly-authentication"></a>Autenticación de WebAssembly de Blazor

En las aplicaciones WebAssembly de Blazor, las comprobaciones de autenticación pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente. Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.

En las secciones siguientes se trata la implementación de un servicio `AuthenticationStateProvider` personalizado para aplicaciones WebAssembly de Blazor.

## <a name="authenticationstateprovider-service"></a>Servicio AuthenticationStateProvider

Las aplicaciones de servidor Blazor incluyen un servicio `AuthenticationStateProvider` integrado que obtiene los datos de estado de autenticación de `HttpContext.User` de ASP.NET Core. Así es como el estado de autenticación se integra con los mecanismos de autenticación existentes en el lado servidor de ASP.NET Core.

`AuthenticationStateProvider` es el servicio subyacente utilizado por el componente `AuthorizeView` y el componente `CascadingAuthenticationState` para obtener el estado de autenticación.

Por lo general, no se utiliza `AuthenticationStateProvider` directamente. Use los enfoques del [componente AuthorizeView](#authorizeview-component) o [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) descritos más adelante en este artículo. El principal inconveniente de utilizar `AuthenticationStateProvider` directamente es que el componente no se notifica de manera automática si cambia el estado de autenticación subyacente de los datos.

El servicio `AuthenticationStateProvider` puede proporcionar los datos <xref:System.Security.Claims.ClaimsPrincipal> del usuario actual, como se muestra en el ejemplo siguiente:

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

Si `user.Identity.IsAuthenticated` es `true` y porque el usuario es <xref:System.Security.Claims.ClaimsPrincipal>, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.

Para más información sobre la inserción de dependencias (DI) y servicios, consulte <xref:blazor/dependency-injection> y <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Implementación de un componente AuthenticationStateProvider personalizado

Si va a crear una aplicación WebAssembly de Blazor o si la especificación de la aplicación requiere absolutamente un proveedor personalizado, implemente un proveedor y reemplace `GetAuthenticationStateAsync`:

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
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

El servicio `CustomAuthStateProvider` se registra en `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Mediante `CustomAuthStateProvider`, todos los usuarios se autentican con el nombre de usuario `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Exposición del estado de autenticación como un parámetro en cascada

Si se requieren los datos de estado de autenticación para la lógica de procedimiento, como cuando se realiza una acción desencadenada por el usuario, obtenga los datos de estado de autenticación mediante la definición de un parámetro en cascada del tipo `Task<AuthenticationState>`:

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

Si `user.Identity.IsAuthenticated` es `true`, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.

Configure el parámetro `Task<AuthenticationState>` en cascada mediante los componentes `AuthorizeRouteView` y `CascadingAuthenticationState`:

```cshtml
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

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

También puede proporcionar contenido diferente para mostrar si el usuario no está autenticado:

```cshtml
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

El contenido de las etiquetas `<Authorized>` y `<NotAuthorized>` puede incluir elementos arbitrarios, como otros componentes interactivos.

Las condiciones de autorización, como los roles o directivas que controlan las opciones o el acceso a la interfaz de usuario, se tratan en la sección [Autorización](#authorization).

Si no se especifican las condiciones de la autorización, `AuthorizeView` usa una directiva predeterminada y trata:

* A los usuarios autenticados (con sesión iniciada) como autorizados.
* A los usuarios no autenticados (sin sesión no iniciada) como no autorizados.

### <a name="role-based-and-policy-based-authorization"></a>Autorización basada en roles y en directivas

El componente `AuthorizeView` admite la autorización *basada en roles* o *basada en directivas*.

Para la autorización basada en roles, utilice el parámetro `Roles`:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Para más información, consulte <xref:security/authorization/roles>.

Para la autorización basada en directivas, utilice el parámetro `Policy`:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

La autorización basada en notificaciones es un caso especial de autorización basada en directivas. Por ejemplo, puede definir una directiva que requiere que los usuarios tengan una notificación determinada. Para más información, consulte <xref:security/authorization/policies>.

Estas API se pueden usar en las aplicaciones de servidor Blazor o en las aplicaciones WebAssembly de Blazor.

Si no se especifica `Roles` ni `Policy`, `AuthorizeView` usa la directiva predeterminada.

### <a name="content-displayed-during-asynchronous-authentication"></a>Contenido que se muestra durante la autenticación asincrónica

Blazor permite que el estado de autenticación se determine *asincrónicamente*. El escenario principal de este enfoque se encuentra en las aplicaciones WebAssembly de Blazor que realizan una solicitud de autenticación a un punto de conexión externo.

Mientras la autenticación está en curso, `AuthorizeView` no muestra ningún contenido de forma predeterminada. Para mostrar el contenido mientras tiene lugar la autenticación, use el elemento `<Authorizing>`:

```cshtml
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

Este enfoque no se aplica normalmente a las aplicaciones de servidor Blazor. Estas aplicaciones conocen el estado de autenticación tan pronto como se establece dicho estado. El contenido `Authorizing` puede proporcionarse en el componente `AuthorizeView` de una aplicación de servidor Blazor, pero nunca se muestra.

## <a name="authorize-attribute"></a>Atributo [Authorize]

Al igual que una aplicación puede usar `[Authorize]` con un controlador de MVC o la página de Razor, `[Authorize]` también se puede usar con los componentes de Razor:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Utilice únicamente `[Authorize]` en componentes `@page` a los que se llega a través del enrutador de Blazor. La autorización solo se realiza como un aspecto del enrutamiento y *no* para los componentes secundarios representados dentro de una página. Para autorizar la presentación de partes concretas dentro de una página, use `AuthorizeView` en su lugar.

Es posible que deba agregar `@using Microsoft.AspNetCore.Authorization` al componente o al archivo *_Imports.razor* para que se compile el componente.

El atributo `[Authorize]` admite también la autorización basada en roles o basada en directivas. Para la autorización basada en roles, utilice el parámetro `Roles`:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Para la autorización basada en directivas, utilice el parámetro `Policy`:

```cshtml
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
* El usuario produce un error en la condición `[Authorize]` aplicada al componente. El atributo `[Authorize]` se trata en la sección [Atributo [Authorize]](#authorize-attribute).
* La autenticación asincrónica está en curso.

En la plantilla de proyecto predeterminada del servidor Blazor, el archivo *App.razor* muestra cómo configurar el contenido personalizado:

```cshtml
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

Si la aplicación determina que los datos de estado de autenticación subyacentes han cambiado (por ejemplo, porque el usuario ha cerrado sesión o porque otro usuario ha cambiado sus roles), un `AuthenticationStateProvider` personalizado puede opcionalmente invocar el método `NotifyAuthenticationStateChanged` en la clase base `AuthenticationStateProvider`. Esto notifica a los consumidores de los datos de estado de autenticación (por ejemplo, `AuthorizeView`) para que los vuelvan a procesar utilizando los nuevos datos.

## <a name="procedural-logic"></a>Lógica de procedimientos

Si se requiere que la aplicación compruebe las reglas de autorización como parte de la lógica de procedimiento, utilice un parámetro en cascada del tipo `Task<AuthenticationState>` para obtener el <xref:System.Security.Claims.ClaimsPrincipal> del usuario. `Task<AuthenticationState>` se puede combinar con otros servicios, como `IAuthorizationService`, para evaluar las directivas.

```cshtml
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

## <a name="authorization-in-blazor-webassembly-apps"></a>Autorización en aplicaciones WebAssembly de Blazor

En aplicaciones WebAssembly de Blazor, las comprobaciones de autorización pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente. Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.

**Realice siempre las comprobaciones de autorización en el servidor dentro de cualquier punto de conexión de la API al que acceda su aplicación del lado cliente.**

## <a name="troubleshoot-errors"></a>Solución de errores

Errores comunes:

* **La autorización requiere un parámetro en casada de tipo Task\<AuthenticationState>. Considere el uso de CascadingAuthenticationState para proporcionarla.**

* **Se recibe el valor `null` para `authenticationStateTask`**

Es probable que el proyecto no se haya creado mediante una plantilla de servidor de Blazor con la autenticación habilitada. Encapsule un `<CascadingAuthenticationState>` alrededor de alguna parte del árbol de la interfaz, por ejemplo, en *App.razor* de la siguiente manera:

```cshtml
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
