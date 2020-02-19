---
title: Autenticación y autorización de ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre los escenarios de autenticación y autorización de Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: c07ffdbd5df58d6b3d19a5d75ce224d830101eac
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447430"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="49161-103">Autenticación y autorización de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="49161-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="49161-104">Por [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="49161-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="49161-105">ASP.NET Core admite la configuración y administración de seguridad en las aplicaciones Blazor.</span><span class="sxs-lookup"><span data-stu-id="49161-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="49161-106">Los escenarios de seguridad varían según si las aplicaciones son de servidor Blazor o WebAssembly de Blazor.</span><span class="sxs-lookup"><span data-stu-id="49161-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="49161-107">Debido a que las aplicaciones de servidor Blazor se ejecutan en el servidor, las comprobaciones de autorización pueden determinar:</span><span class="sxs-lookup"><span data-stu-id="49161-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="49161-108">Las opciones de la interfaz de usuario presentadas a un usuario (por ejemplo, qué entradas de menú están disponibles para el usuario).</span><span class="sxs-lookup"><span data-stu-id="49161-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="49161-109">Las reglas de acceso para las áreas de la aplicación y los componentes.</span><span class="sxs-lookup"><span data-stu-id="49161-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="49161-110">Las aplicaciones WebAssembly de Blazor se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="49161-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="49161-111">La autorización *solo* se utiliza para determinar qué opciones de la interfaz de usuario se van a mostrar.</span><span class="sxs-lookup"><span data-stu-id="49161-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="49161-112">Dado que el usuario puede modificar u omitir las comprobaciones en el cliente, las aplicaciones WebAssembly de Blazor no puede aplicar reglas de acceso de autorización.</span><span class="sxs-lookup"><span data-stu-id="49161-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="49161-113">Autenticación</span><span class="sxs-lookup"><span data-stu-id="49161-113">Authentication</span></span>

<span data-ttu-id="49161-114">Blazor utiliza los mecanismos de autenticación de ASP.NET Core existentes para establecer la identidad del usuario.</span><span class="sxs-lookup"><span data-stu-id="49161-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="49161-115">El mecanismo exacto depende de la forma en que se hospeda la aplicación: servidor Blazor o WebAssembly de Blazor.</span><span class="sxs-lookup"><span data-stu-id="49161-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="49161-116">Autenticación del servidor Blazor</span><span class="sxs-lookup"><span data-stu-id="49161-116">Blazor Server authentication</span></span>

<span data-ttu-id="49161-117">Las aplicaciones de servidor Blazor funcionan mediante una conexión en tiempo real que se crea con SignalR.</span><span class="sxs-lookup"><span data-stu-id="49161-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="49161-118">[La autenticación en aplicaciones basadas en SignalR](xref:signalr/authn-and-authz) se controla cuando se establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="49161-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="49161-119">La autenticación se puede basar en una cookie o en cualquier otro token de portador.</span><span class="sxs-lookup"><span data-stu-id="49161-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="49161-120">La plantilla de proyecto de servidor Blazor puede configurar la autenticación cuando se crea el proyecto.</span><span class="sxs-lookup"><span data-stu-id="49161-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="49161-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49161-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="49161-122">Siga las instrucciones de Visual Studio que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="49161-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="49161-123">Después de elegir la plantilla **Aplicación de servidor Blazor** en el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **Cambiar** en **Autenticación**.</span><span class="sxs-lookup"><span data-stu-id="49161-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="49161-124">Se abre un cuadro de diálogo para ofrecer el mismo conjunto de mecanismos de autenticación disponibles para otros proyectos ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="49161-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="49161-125">**Sin autenticación**</span><span class="sxs-lookup"><span data-stu-id="49161-125">**No Authentication**</span></span>
* <span data-ttu-id="49161-126">**Cuentas de usuario individuales** &ndash;Las cuentas de usuario se pueden almacenar:</span><span class="sxs-lookup"><span data-stu-id="49161-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="49161-127">Dentro de la aplicación mediante el sistema de [identidad](xref:security/authentication/identity) de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49161-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="49161-128">Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c)</span><span class="sxs-lookup"><span data-stu-id="49161-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="49161-129">**Cuentas profesionales o educativas**</span><span class="sxs-lookup"><span data-stu-id="49161-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="49161-130">**Autenticación de Windows**</span><span class="sxs-lookup"><span data-stu-id="49161-130">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="49161-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="49161-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="49161-132">Siga las instrucciones de Visual Studio Code que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación:</span><span class="sxs-lookup"><span data-stu-id="49161-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="49161-133">En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="49161-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="49161-134">Mecanismo de autenticación</span><span class="sxs-lookup"><span data-stu-id="49161-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="49161-135">Valor de`{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="49161-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="49161-136">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="49161-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="49161-137">Individual</span><span class="sxs-lookup"><span data-stu-id="49161-137">Individual</span></span><br><span data-ttu-id="49161-138">Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49161-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="49161-139">Individual</span><span class="sxs-lookup"><span data-stu-id="49161-139">Individual</span></span><br><span data-ttu-id="49161-140">Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="49161-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="49161-141">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="49161-141">Work or School Accounts</span></span><br><span data-ttu-id="49161-142">Autenticación organizativa para un solo inquilino.</span><span class="sxs-lookup"><span data-stu-id="49161-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="49161-143">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="49161-143">Work or School Accounts</span></span><br><span data-ttu-id="49161-144">Autenticación organizativa para varios inquilinos.</span><span class="sxs-lookup"><span data-stu-id="49161-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="49161-145">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="49161-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="49161-146">El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49161-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="49161-147">Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="49161-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
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

### <a name="opno-locblazor-webassembly-authentication"></a><span data-ttu-id="49161-148">Autenticación de Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="49161-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="49161-149">En las aplicaciones de Blazor WebAssembly, las comprobaciones de autenticación pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="49161-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="49161-150">Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="49161-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="49161-151">Agregue una referencia de paquete para [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) al archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49161-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="49161-152">En las secciones siguientes se trata la implementación de un servicio `AuthenticationStateProvider` personalizado para aplicaciones de Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="49161-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="49161-153">Servicio AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="49161-153">AuthenticationStateProvider service</span></span>

<span data-ttu-id="49161-154">Las aplicaciones del servidor de Blazor incluyen un servicio `AuthenticationStateProvider` integrado que obtiene los datos de estado de autenticación de `HttpContext.User` de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49161-154">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="49161-155">Así es como el estado de autenticación se integra con los mecanismos de autenticación existentes en el lado servidor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49161-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="49161-156">`AuthenticationStateProvider` es el servicio subyacente utilizado por el componente `AuthorizeView` y el componente `CascadingAuthenticationState` para obtener el estado de autenticación.</span><span class="sxs-lookup"><span data-stu-id="49161-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="49161-157">Por lo general, no se utiliza `AuthenticationStateProvider` directamente.</span><span class="sxs-lookup"><span data-stu-id="49161-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="49161-158">Use los enfoques del [componente AuthorizeView](#authorizeview-component) o [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) descritos más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="49161-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="49161-159">El principal inconveniente de utilizar `AuthenticationStateProvider` directamente es que el componente no se notifica de manera automática si cambia el estado de autenticación subyacente de los datos.</span><span class="sxs-lookup"><span data-stu-id="49161-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="49161-160">El servicio `AuthenticationStateProvider` puede proporcionar los datos <xref:System.Security.Claims.ClaimsPrincipal> del usuario actual, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="49161-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

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

<span data-ttu-id="49161-161">Si `user.Identity.IsAuthenticated` es `true` y porque el usuario es <xref:System.Security.Claims.ClaimsPrincipal>, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.</span><span class="sxs-lookup"><span data-stu-id="49161-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="49161-162">Para más información sobre la inserción de dependencias (DI) y servicios, consulte <xref:blazor/dependency-injection> y <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="49161-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="49161-163">Implementación de un componente AuthenticationStateProvider personalizado</span><span class="sxs-lookup"><span data-stu-id="49161-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="49161-164">Si va a compilar una aplicación de Blazor WebAssembly o la especificación de la aplicación requiere un proveedor personalizado, implemente un proveedor y reemplace `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="49161-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
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
}
```

<span data-ttu-id="49161-165">En una aplicación WebAssembly de Blazor, el servicio `CustomAuthStateProvider` se registra en `Main` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="49161-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="49161-166">Mediante `CustomAuthStateProvider`, todos los usuarios se autentican con el nombre de usuario `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="49161-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="49161-167">Exposición del estado de autenticación como un parámetro en cascada</span><span class="sxs-lookup"><span data-stu-id="49161-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="49161-168">Si se requieren los datos de estado de autenticación para la lógica de procedimiento, como cuando se realiza una acción desencadenada por el usuario, obtenga los datos de estado de autenticación mediante la definición de un parámetro en cascada del tipo `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="49161-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

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

> [!NOTE]
> <span data-ttu-id="49161-169">En un componente de aplicación de Blazor WebAssembly, agregue el espacio de nombres `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="49161-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="49161-170">Si `user.Identity.IsAuthenticated` es `true`, se pueden enumerar las notificaciones y evaluar la pertenencia a roles.</span><span class="sxs-lookup"><span data-stu-id="49161-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="49161-171">Configure el parámetro en cascada `Task<AuthenticationState>` mediante los componentes `AuthorizeRouteView` y `CascadingAuthenticationState` en el archivo *App.razor*:</span><span class="sxs-lookup"><span data-stu-id="49161-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization

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

<span data-ttu-id="49161-172">Agregue servicios relativos a las opciones y la autorización para `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="49161-172">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="49161-173">Autorización</span><span class="sxs-lookup"><span data-stu-id="49161-173">Authorization</span></span>

<span data-ttu-id="49161-174">Cuando un usuario está autenticado, se aplican las reglas de *autorización* para controlar qué puede hacer el usuario.</span><span class="sxs-lookup"><span data-stu-id="49161-174">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="49161-175">Por lo general, se concede o deniega el acceso en función de si:</span><span class="sxs-lookup"><span data-stu-id="49161-175">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="49161-176">El usuario está autenticado (ha iniciado sesión).</span><span class="sxs-lookup"><span data-stu-id="49161-176">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="49161-177">El usuario está en un *rol*.</span><span class="sxs-lookup"><span data-stu-id="49161-177">A user is in a *role*.</span></span>
* <span data-ttu-id="49161-178">El usuario tiene una *notificación*.</span><span class="sxs-lookup"><span data-stu-id="49161-178">A user has a *claim*.</span></span>
* <span data-ttu-id="49161-179">Una *directiva* se cumple.</span><span class="sxs-lookup"><span data-stu-id="49161-179">A *policy* is satisfied.</span></span>

<span data-ttu-id="49161-180">Cada uno de estos conceptos es el mismo que en una aplicación ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="49161-180">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="49161-181">Para más información sobre la seguridad de ASP.NET Core, consulte los artículos que se encuentran en [ASP.NET Core Security and Identity](xref:security/index) (Identidad y seguridad de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="49161-181">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="49161-182">Componente AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="49161-182">AuthorizeView component</span></span>

<span data-ttu-id="49161-183">El componente `AuthorizeView` selectivamente muestra la interfaz de usuario dependiendo de si el usuario está autorizado para verlo.</span><span class="sxs-lookup"><span data-stu-id="49161-183">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="49161-184">Este enfoque es útil cuando solo necesite *mostrar* datos del usuario y no es necesario usar la identidad del usuario en la lógica de procedimientos.</span><span class="sxs-lookup"><span data-stu-id="49161-184">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="49161-185">El componente expone una variable `context` del tipo `AuthenticationState`, que puede utilizar para acceder a la información sobre el usuario que ha iniciado sesión:</span><span class="sxs-lookup"><span data-stu-id="49161-185">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="49161-186">También puede proporcionar contenido diferente para mostrar si el usuario no está autenticado:</span><span class="sxs-lookup"><span data-stu-id="49161-186">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="49161-187">El contenido de las etiquetas `<Authorized>` y `<NotAuthorized>` puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="49161-187">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="49161-188">Las condiciones de autorización, como los roles o directivas que controlan las opciones o el acceso a la interfaz de usuario, se tratan en la sección [Autorización](#authorization).</span><span class="sxs-lookup"><span data-stu-id="49161-188">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="49161-189">Si no se especifican las condiciones de la autorización, `AuthorizeView` usa una directiva predeterminada y trata:</span><span class="sxs-lookup"><span data-stu-id="49161-189">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="49161-190">A los usuarios autenticados (con sesión iniciada) como autorizados.</span><span class="sxs-lookup"><span data-stu-id="49161-190">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="49161-191">A los usuarios no autenticados (sin sesión no iniciada) como no autorizados.</span><span class="sxs-lookup"><span data-stu-id="49161-191">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="49161-192">Autorización basada en roles y en directivas</span><span class="sxs-lookup"><span data-stu-id="49161-192">Role-based and policy-based authorization</span></span>

<span data-ttu-id="49161-193">El componente `AuthorizeView` admite la autorización *basada en roles* o *basada en directivas*.</span><span class="sxs-lookup"><span data-stu-id="49161-193">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="49161-194">Para la autorización basada en roles, utilice el parámetro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="49161-194">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="49161-195">Para obtener más información, vea <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="49161-195">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="49161-196">Para la autorización basada en directivas, utilice el parámetro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="49161-196">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="49161-197">La autorización basada en notificaciones es un caso especial de autorización basada en directivas.</span><span class="sxs-lookup"><span data-stu-id="49161-197">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="49161-198">Por ejemplo, puede definir una directiva que requiere que los usuarios tengan una notificación determinada.</span><span class="sxs-lookup"><span data-stu-id="49161-198">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="49161-199">Para obtener más información, vea <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="49161-199">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="49161-200">Estas API se pueden usar en las aplicaciones del servidor de Blazor o Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="49161-200">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="49161-201">Si no se especifica `Roles` ni `Policy`, `AuthorizeView` usa la directiva predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49161-201">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="49161-202">Contenido que se muestra durante la autenticación asincrónica</span><span class="sxs-lookup"><span data-stu-id="49161-202">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="49161-203"> permite que el estado de autenticación se determine *asincrónicamente*.</span><span class="sxs-lookup"><span data-stu-id="49161-203"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="49161-204">El escenario principal de este enfoque se encuentra en las aplicaciones de Blazor WebAssembly que realizan una solicitud de autenticación a un punto de conexión externo.</span><span class="sxs-lookup"><span data-stu-id="49161-204">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="49161-205">Mientras la autenticación está en curso, `AuthorizeView` no muestra ningún contenido de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="49161-205">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="49161-206">Para mostrar el contenido mientras tiene lugar la autenticación, use el elemento `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="49161-206">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="49161-207">Este enfoque no se aplica normalmente a las aplicaciones del servidor de Blazor.</span><span class="sxs-lookup"><span data-stu-id="49161-207">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="49161-208">Las aplicaciones del servidor de Blazor conocen el estado de autenticación tan pronto como se establece dicho estado.</span><span class="sxs-lookup"><span data-stu-id="49161-208">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="49161-209">El contenido `Authorizing` puede proporcionarse en el componente `AuthorizeView` de una aplicación del servidor de Blazor, pero nunca se muestra.</span><span class="sxs-lookup"><span data-stu-id="49161-209">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="49161-210">Atributo [Authorize]</span><span class="sxs-lookup"><span data-stu-id="49161-210">[Authorize] attribute</span></span>

<span data-ttu-id="49161-211">El atributo `[Authorize]` se puede usar en los componentes de Razor:</span><span class="sxs-lookup"><span data-stu-id="49161-211">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="49161-212">En un componente de aplicación de Blazor WebAssembly, agregue el espacio de nombres `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) a los ejemplos de esta sección.</span><span class="sxs-lookup"><span data-stu-id="49161-212">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49161-213">Utilice únicamente `[Authorize]` en componentes `@page` a los que se llega a través del enrutador de Blazor.</span><span class="sxs-lookup"><span data-stu-id="49161-213">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="49161-214">La autorización solo se realiza como un aspecto del enrutamiento y *no* para los componentes secundarios representados dentro de una página.</span><span class="sxs-lookup"><span data-stu-id="49161-214">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="49161-215">Para autorizar la presentación de partes concretas dentro de una página, use `AuthorizeView` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="49161-215">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="49161-216">El atributo `[Authorize]` admite también la autorización basada en roles o basada en directivas.</span><span class="sxs-lookup"><span data-stu-id="49161-216">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="49161-217">Para la autorización basada en roles, utilice el parámetro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="49161-217">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="49161-218">Para la autorización basada en directivas, utilice el parámetro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="49161-218">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="49161-219">Si no se especifica `Roles` ni `Policy`, `[Authorize]` usa la directiva predeterminada, que consiste en tratar:</span><span class="sxs-lookup"><span data-stu-id="49161-219">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="49161-220">A los usuarios autenticados (con sesión iniciada) como autorizados.</span><span class="sxs-lookup"><span data-stu-id="49161-220">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="49161-221">A los usuarios no autenticados (sin sesión no iniciada) como no autorizados.</span><span class="sxs-lookup"><span data-stu-id="49161-221">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="49161-222">Personalización del contenido no autorizado con el componente de enrutador</span><span class="sxs-lookup"><span data-stu-id="49161-222">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="49161-223">El componente `Router`, junto con el componente `AuthorizeRouteView`, permite que la aplicación especifique el contenido personalizado si:</span><span class="sxs-lookup"><span data-stu-id="49161-223">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="49161-224">No se encuentra el contenido.</span><span class="sxs-lookup"><span data-stu-id="49161-224">Content isn't found.</span></span>
* <span data-ttu-id="49161-225">El usuario produce un error en la condición `[Authorize]` aplicada al componente.</span><span class="sxs-lookup"><span data-stu-id="49161-225">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="49161-226">El atributo `[Authorize]` se describe en la sección [Atributo `[Authorize]`](#authorize-attribute).</span><span class="sxs-lookup"><span data-stu-id="49161-226">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="49161-227">La autenticación asincrónica está en curso.</span><span class="sxs-lookup"><span data-stu-id="49161-227">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="49161-228">En la plantilla de proyecto predeterminada del servidor de Blazor, el archivo *App.razor* muestra cómo configurar el contenido personalizado:</span><span class="sxs-lookup"><span data-stu-id="49161-228">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="49161-229">El contenido de las etiquetas `<NotFound>`, `<NotAuthorized>` y `<Authorizing>` puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="49161-229">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="49161-230">Si el elemento `<NotAuthorized>` no se especifica, `AuthorizeRouteView` utiliza el siguiente mensaje de reserva:</span><span class="sxs-lookup"><span data-stu-id="49161-230">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="49161-231">Notificación sobre los cambios de estado de autenticación</span><span class="sxs-lookup"><span data-stu-id="49161-231">Notification about authentication state changes</span></span>

<span data-ttu-id="49161-232">Si la aplicación determina que los datos de estado de autenticación subyacentes han cambiado (por ejemplo, porque el usuario ha cerrado sesión o porque otro usuario ha cambiado sus roles), un `AuthenticationStateProvider` personalizado puede opcionalmente invocar el método `NotifyAuthenticationStateChanged` en la clase base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="49161-232">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="49161-233">Esto notifica a los consumidores de los datos de estado de autenticación (por ejemplo, `AuthorizeView`) para que los vuelvan a procesar utilizando los nuevos datos.</span><span class="sxs-lookup"><span data-stu-id="49161-233">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="49161-234">Lógica de procedimientos</span><span class="sxs-lookup"><span data-stu-id="49161-234">Procedural logic</span></span>

<span data-ttu-id="49161-235">Si se requiere que la aplicación compruebe las reglas de autorización como parte de la lógica de procedimiento, utilice un parámetro en cascada del tipo `Task<AuthenticationState>` para obtener el <xref:System.Security.Claims.ClaimsPrincipal> del usuario.</span><span class="sxs-lookup"><span data-stu-id="49161-235">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="49161-236">`Task<AuthenticationState>` se puede combinar con otros servicios, como `IAuthorizationService`, para evaluar las directivas.</span><span class="sxs-lookup"><span data-stu-id="49161-236">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="49161-237">En un componente de la aplicación de Blazor WebAssembly, agregue los espacios de nombres `Microsoft.AspNetCore.Authorization` y `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="49161-237">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="49161-238">Autorización en aplicaciones de Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="49161-238">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="49161-239">En aplicaciones de Blazor WebAssembly, las comprobaciones de autorización pueden omitirse porque los usuarios pueden modificar todos los códigos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="49161-239">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="49161-240">Lo mismo se aplica a todas las tecnologías de aplicaciones del lado cliente, incluidas las plataformas JavaScript SPA o las aplicaciones nativas para cualquier sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="49161-240">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="49161-241">**Realice siempre las comprobaciones de autorización en el servidor dentro de cualquier punto de conexión de la API al que acceda su aplicación del lado cliente.**</span><span class="sxs-lookup"><span data-stu-id="49161-241">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="49161-242">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="49161-242">Troubleshoot errors</span></span>

<span data-ttu-id="49161-243">Errores comunes:</span><span class="sxs-lookup"><span data-stu-id="49161-243">Common errors:</span></span>

* <span data-ttu-id="49161-244">**La autorización requiere un parámetro en casada de tipo Task\<AuthenticationState>. Considere el uso de CascadingAuthenticationState para proporcionarla.**</span><span class="sxs-lookup"><span data-stu-id="49161-244">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="49161-245">**Se recibe el valor `null` para `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="49161-245">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="49161-246">Es probable que el proyecto no se haya creado mediante una plantilla del servidor de Blazor con la autenticación habilitada.</span><span class="sxs-lookup"><span data-stu-id="49161-246">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="49161-247">Encapsule un `<CascadingAuthenticationState>` alrededor de alguna parte del árbol de la interfaz, por ejemplo, en *App.razor* de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="49161-247">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="49161-248">El `CascadingAuthenticationState` proporciona el parámetro en cascada `Task<AuthenticationState>`, que a su vez recibe el servicio DI `AuthenticationStateProvider` subyacente.</span><span class="sxs-lookup"><span data-stu-id="49161-248">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49161-249">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="49161-249">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="49161-250">[GenialBlazor: Vínculos de ejemplo de la comunidad de autenticación](https://github.com/AdrienTorris/awesome-blazor#authentication)</span><span class="sxs-lookup"><span data-stu-id="49161-250">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
