---
title: Aplicaciones seguras Blazor de ASP.NET Core Server
author: guardrex
description: Obtenga información sobre cómo Blazor mitigar las amenazas de seguridad de las aplicaciones de servidor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: bd03f811d0425fdfdb7bbbc24fea5481b49b8ed3
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80626016"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="2ccf5-103">Aplicaciones seguras ASP.NET Core Blazor Server</span><span class="sxs-lookup"><span data-stu-id="2ccf5-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="2ccf5-104">Por [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

<span data-ttu-id="2ccf5-105">Las aplicaciones de Blazor Server adoptan un modelo de procesamiento de datos *con estado,* donde el servidor y el cliente mantienen una relación de larga duración.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-105">Blazor Server apps adopt a *stateful* data processing model, where the server and client maintain a long-lived relationship.</span></span> <span data-ttu-id="2ccf5-106">El estado persistente es mantenido por un [circuito,](xref:blazor/state-management)que puede abarcar conexiones que también son potencialmente longevas.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-106">The persistent state is maintained by a [circuit](xref:blazor/state-management), which can span connections that are also potentially long-lived.</span></span>

<span data-ttu-id="2ccf5-107">Cuando un usuario visita un sitio de Blazor Server, el servidor crea un circuito en la memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-107">When a user visits a Blazor Server site, the server creates a circuit in the server's memory.</span></span> <span data-ttu-id="2ccf5-108">El circuito indica al explorador qué contenido representar y responde a eventos, como cuando el usuario selecciona un botón en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-108">The circuit indicates to the browser what content to render and responds to events, such as when the user selects a button in the UI.</span></span> <span data-ttu-id="2ccf5-109">Para realizar estas acciones, un circuito invoca funciones JavaScript en el explorador del usuario y métodos .NET en el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-109">To perform these actions, a circuit invokes JavaScript functions in the user's browser and .NET methods on the server.</span></span> <span data-ttu-id="2ccf5-110">Esta interacción bidireccional basada en JavaScript se conoce como [interoperabilidad de JavaScript (interoperabilidad JS).](xref:blazor/call-javascript-from-dotnet)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-110">This two-way JavaScript-based interaction is referred to as [JavaScript interop (JS interop)](xref:blazor/call-javascript-from-dotnet).</span></span>

<span data-ttu-id="2ccf5-111">Dado que la interoperabilidad de JS se produce a través de Internet y el cliente utiliza un explorador remoto, las aplicaciones de Blazor Server comparten la mayoría de los problemas de seguridad de las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-111">Because JS interop occurs over the Internet and the client uses a remote browser, Blazor Server apps share most web app security concerns.</span></span> <span data-ttu-id="2ccf5-112">En este tema se describen las amenazas comunes a las aplicaciones de Blazor Server y se proporcionan instrucciones sobre la mitigación de amenazas centradas en las aplicaciones orientadas a Internet.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-112">This topic describes common threats to Blazor Server apps and provides threat mitigation guidance focused on Internet-facing apps.</span></span>

<span data-ttu-id="2ccf5-113">En entornos restringidos, como dentro de redes corporativas o intranets, algunas de las instrucciones de mitigación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-113">In constrained environments, such as inside corporate networks or intranets, some of the mitigation guidance either:</span></span>

* <span data-ttu-id="2ccf5-114">No se aplica en el entorno restringido.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-114">Doesn't apply in the constrained environment.</span></span>
* <span data-ttu-id="2ccf5-115">No vale la pena el costo de implementar porque el riesgo de seguridad es bajo en un entorno restringido.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-115">Isn't worth the cost to implement because the security risk is low in a constrained environment.</span></span>

## <a name="blazor-server-project-template"></a><span data-ttu-id="2ccf5-116">Plantilla de proyecto de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="2ccf5-116">Blazor Server project template</span></span>

<span data-ttu-id="2ccf5-117">La plantilla de proyecto DeBlazor Server se puede configurar para la autenticación cuando se crea el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-117">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2ccf5-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ccf5-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2ccf5-119">Siga las instrucciones de Visual Studio que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-119">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="2ccf5-120">Después de elegir la plantilla **Aplicación de servidor Blazor** en el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **Cambiar** en **Autenticación**.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-120">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="2ccf5-121">Se abre un cuadro de diálogo para ofrecer el mismo conjunto de mecanismos de autenticación disponibles para otros proyectos ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-121">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="2ccf5-122">**Sin autenticación**</span><span class="sxs-lookup"><span data-stu-id="2ccf5-122">**No Authentication**</span></span>
* <span data-ttu-id="2ccf5-123">**Cuentas de usuario individuales** &ndash;Las cuentas de usuario se pueden almacenar:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-123">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="2ccf5-124">Dentro de la aplicación mediante el sistema de [identidad](xref:security/authentication/identity) de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-124">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="2ccf5-125">Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-125">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="2ccf5-126">**Cuentas de trabajo o escolares**</span><span class="sxs-lookup"><span data-stu-id="2ccf5-126">**Work or School Accounts**</span></span>
* <span data-ttu-id="2ccf5-127">**Autenticación de Windows**</span><span class="sxs-lookup"><span data-stu-id="2ccf5-127">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2ccf5-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ccf5-128">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2ccf5-129">Siga las instrucciones de Visual Studio Code que se indican en el artículo <xref:blazor/get-started> para crear un proyecto de servidor Blazor con un mecanismo de autenticación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-129">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="2ccf5-130">En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-130">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="2ccf5-131">Mecanismo de autenticación</span><span class="sxs-lookup"><span data-stu-id="2ccf5-131">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="2ccf5-132">Valor de `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="2ccf5-132">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="2ccf5-133">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="2ccf5-133">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="2ccf5-134">Individual</span><span class="sxs-lookup"><span data-stu-id="2ccf5-134">Individual</span></span><br><span data-ttu-id="2ccf5-135">Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-135">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="2ccf5-136">Individual</span><span class="sxs-lookup"><span data-stu-id="2ccf5-136">Individual</span></span><br><span data-ttu-id="2ccf5-137">Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-137">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="2ccf5-138">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-138">Work or School Accounts</span></span><br><span data-ttu-id="2ccf5-139">Autenticación organizativa para un solo inquilino.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-139">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="2ccf5-140">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-140">Work or School Accounts</span></span><br><span data-ttu-id="2ccf5-141">Autenticación organizativa para varios inquilinos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-141">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="2ccf5-142">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="2ccf5-142">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="2ccf5-143">El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-143">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="2ccf5-144">Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-144">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2ccf5-145">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2ccf5-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2ccf5-146">Siga las instrucciones de Visual <xref:blazor/get-started> Studio para Mac en el artículo.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-146">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="2ccf5-147">En el paso Configurar la nueva aplicación de **servidor de Blazor,** seleccione **Autenticación individual (en la aplicación)** en el menú desplegable **Autenticación.**</span><span class="sxs-lookup"><span data-stu-id="2ccf5-147">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="2ccf5-148">La aplicación se crea para usuarios individuales almacenados en la aplicación con ASP.NET identidad principal.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-148">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2ccf5-149">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2ccf5-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="2ccf5-150">Siga las instrucciones de la <xref:blazor/get-started> CLI de .NET Core del artículo para crear un nuevo proyecto de Blazor Server con un mecanismo de autenticación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-150">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="2ccf5-151">En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-151">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="2ccf5-152">Mecanismo de autenticación</span><span class="sxs-lookup"><span data-stu-id="2ccf5-152">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="2ccf5-153">Valor de `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="2ccf5-153">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="2ccf5-154">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="2ccf5-154">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="2ccf5-155">Individual</span><span class="sxs-lookup"><span data-stu-id="2ccf5-155">Individual</span></span><br><span data-ttu-id="2ccf5-156">Usuarios almacenados en la aplicación con la identidad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-156">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="2ccf5-157">Individual</span><span class="sxs-lookup"><span data-stu-id="2ccf5-157">Individual</span></span><br><span data-ttu-id="2ccf5-158">Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-158">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="2ccf5-159">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-159">Work or School Accounts</span></span><br><span data-ttu-id="2ccf5-160">Autenticación organizativa para un solo inquilino.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-160">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="2ccf5-161">Cuentas profesionales o educativas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-161">Work or School Accounts</span></span><br><span data-ttu-id="2ccf5-162">Autenticación organizativa para varios inquilinos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-162">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="2ccf5-163">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="2ccf5-163">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="2ccf5-164">El comando crea una carpeta con el valor proporcionado para el marcador de posición `{APP NAME}` y usa el nombre de la carpeta como nombre de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-164">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="2ccf5-165">Para más información, consulte el comando [dotnet new](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-165">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="pass-tokens-to-a-blazor-server-app"></a><span data-ttu-id="2ccf5-166">Pasar tokens a una aplicación Blazor Server</span><span class="sxs-lookup"><span data-stu-id="2ccf5-166">Pass tokens to a Blazor Server app</span></span>

<span data-ttu-id="2ccf5-167">Autentique la aplicación Blazor Server como lo haría con una aplicación normal de Razor Pages o MVC.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-167">Authenticate the Blazor Server app as you would with a regular Razor Pages or MVC app.</span></span> <span data-ttu-id="2ccf5-168">Aprovisione y guarde los tokens en la cookie de autenticación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-168">Provision and save the tokens to the authentication cookie.</span></span> <span data-ttu-id="2ccf5-169">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-169">For example:</span></span>

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

<span data-ttu-id="2ccf5-170">Para obtener código de `Startup.ConfigureServices` ejemplo, incluido un ejemplo completo, consulte [Pasar tokens a una aplicación Blazor del lado servidor.](https://github.com/javiercn/blazor-server-aad-sample)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-170">For sample code, including a complete `Startup.ConfigureServices` example, see the [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample).</span></span>

<span data-ttu-id="2ccf5-171">Defina una clase para pasar el estado inicial de la aplicación con los tokens de acceso y actualización:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-171">Define a class to pass in the initial app state with the access and refresh tokens:</span></span>

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="2ccf5-172">Defina un servicio de proveedor de tokens **con ámbito** que se pueda usar dentro de la aplicación Blazor para resolver los tokens desde DI:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-172">Define a **scoped** token provider service that can be used within the Blazor app to resolve the tokens from DI:</span></span>

```csharp
using System;
using System.Security.Claims;
using System.Threading.Tasks;

public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="2ccf5-173">En `Startup.ConfigureServices`, agregue servicios para:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-173">In `Startup.ConfigureServices`, add services for:</span></span>

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

<span data-ttu-id="2ccf5-174">En el archivo *_Host.cshtml,* `InitialApplicationState` cree e instancia y páselo como parámetro a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-174">In the *_Host.cshtml* file, create and instance of `InitialApplicationState` and pass it as a parameter to the app:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

<span data-ttu-id="2ccf5-175">En `App` el componente (*App.razor*), resuelva el servicio e inicialícelo con los datos del parámetro:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-175">In the `App` component (*App.razor*), resolve the service and initialize it with the data from the parameter:</span></span>

```razor
@inject TokenProvider TokensProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokensProvider.AccessToken = InitialState.AccessToken;
        TokensProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

<span data-ttu-id="2ccf5-176">En el servicio que realiza una solicitud de API segura, inserte el proveedor de tokens y recupere el token para llamar a la API:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-176">In the service that makes a secure API request, inject the token provider and retrieve the token to call the API:</span></span>

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="resource-exhaustion"></a><span data-ttu-id="2ccf5-177">Agotamiento de recursos</span><span class="sxs-lookup"><span data-stu-id="2ccf5-177">Resource exhaustion</span></span>

<span data-ttu-id="2ccf5-178">El agotamiento de recursos puede producirse cuando un cliente interactúa con el servidor y hace que el servidor consuma recursos excesivos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-178">Resource exhaustion can occur when a client interacts with the server and causes the server to consume excessive resources.</span></span> <span data-ttu-id="2ccf5-179">El consumo excesivo de recursos afecta principalmente a:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-179">Excessive resource consumption primarily affects:</span></span>

* [<span data-ttu-id="2ccf5-180">Cpu</span><span class="sxs-lookup"><span data-stu-id="2ccf5-180">CPU</span></span>](#cpu)
* [<span data-ttu-id="2ccf5-181">Memoria</span><span class="sxs-lookup"><span data-stu-id="2ccf5-181">Memory</span></span>](#memory)
* [<span data-ttu-id="2ccf5-182">Conexiones de cliente</span><span class="sxs-lookup"><span data-stu-id="2ccf5-182">Client connections</span></span>](#client-connections)

<span data-ttu-id="2ccf5-183">Los ataques de denegación de servicio (DoS) suelen intentar agotar los recursos de una aplicación o servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-183">Denial of service (DoS) attacks usually seek to exhaust an app or server's resources.</span></span> <span data-ttu-id="2ccf5-184">Sin embargo, el agotamiento de recursos no es necesariamente el resultado de un ataque al sistema.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-184">However, resource exhaustion isn't necessarily the result of an attack on the system.</span></span> <span data-ttu-id="2ccf5-185">Por ejemplo, los recursos finitos se pueden agotar debido a la alta demanda de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-185">For example, finite resources can be exhausted due to high user demand.</span></span> <span data-ttu-id="2ccf5-186">DoS se cubre más adelante en la sección [ataques de denegación de servicio (DoS).](#denial-of-service-dos-attacks)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-186">DoS is covered further in the [Denial of service (DoS) attacks](#denial-of-service-dos-attacks) section.</span></span>

<span data-ttu-id="2ccf5-187">Los recursos externos al marco de Blazor, como bases de datos y identificadores de archivos (utilizados para leer y escribir archivos), también pueden experimentar agotamiento de recursos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-187">Resources external to the Blazor framework, such as databases and file handles (used to read and write files), may also experience resource exhaustion.</span></span> <span data-ttu-id="2ccf5-188">Para obtener más información, vea <xref:performance/performance-best-practices>.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-188">For more information, see <xref:performance/performance-best-practices>.</span></span>

### <a name="cpu"></a><span data-ttu-id="2ccf5-189">CPU</span><span class="sxs-lookup"><span data-stu-id="2ccf5-189">CPU</span></span>

<span data-ttu-id="2ccf5-190">El agotamiento de la CPU puede ocurrir cuando uno o más clientes fuerzan al servidor a realizar un trabajo intensivo de CPU.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-190">CPU exhaustion can occur when one or more clients force the server to perform intensive CPU work.</span></span>

<span data-ttu-id="2ccf5-191">Por ejemplo, considere una aplicación Blazor Server que calcula un número de *Fibonnacci.*</span><span class="sxs-lookup"><span data-stu-id="2ccf5-191">For example, consider a Blazor Server app that calculates a *Fibonnacci number*.</span></span> <span data-ttu-id="2ccf5-192">Un número de Fibonnacci se produce a partir de una secuencia de Fibonnacci, donde cada número de la secuencia es la suma de los dos números anteriores.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-192">A Fibonnacci number is produced from a Fibonnacci sequence, where each number in the sequence is the sum of the two preceding numbers.</span></span> <span data-ttu-id="2ccf5-193">La cantidad de trabajo necesario para llegar a la respuesta depende de la longitud de la secuencia y del tamaño del valor inicial.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-193">The amount of work required to reach the answer depends on the length of the sequence and the size of the initial value.</span></span> <span data-ttu-id="2ccf5-194">Si la aplicación no pone límites a la solicitud de un cliente, los cálculos de uso intensivo de CPU pueden dominar el tiempo de la CPU y disminuir el rendimiento de otras tareas.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-194">If the app doesn't place limits on a client's request, the CPU-intensive calculations may dominate the CPU's time and diminish the performance of other tasks.</span></span> <span data-ttu-id="2ccf5-195">El consumo excesivo de recursos es un problema de seguridad que afecta a la disponibilidad.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-195">Excessive resource consumption is a security concern impacting availability.</span></span>

<span data-ttu-id="2ccf5-196">El agotamiento de la CPU es una preocupación para todas las aplicaciones orientadas al público.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-196">CPU exhaustion is a concern for all public-facing apps.</span></span> <span data-ttu-id="2ccf5-197">En las aplicaciones web normales, las solicitudes y conexiones agotan el tiempo de espera como medida de seguridad, pero las aplicaciones de Blazor Server no proporcionan las mismas medidas de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-197">In regular web apps, requests and connections time out as a safeguard, but Blazor Server apps don't provide the same safeguards.</span></span> <span data-ttu-id="2ccf5-198">Las aplicaciones de Blazor Server deben incluir comprobaciones y límites adecuados antes de realizar un trabajo con un uso intensivo de la CPU.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-198">Blazor Server apps must include appropriate checks and limits before performing potentially CPU-intensive work.</span></span>

### <a name="memory"></a><span data-ttu-id="2ccf5-199">Memoria</span><span class="sxs-lookup"><span data-stu-id="2ccf5-199">Memory</span></span>

<span data-ttu-id="2ccf5-200">El agotamiento de la memoria puede producirse cuando uno o más clientes obligan al servidor a consumir una gran cantidad de memoria.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-200">Memory exhaustion can occur when one or more clients force the server to consume a large amount of memory.</span></span>

<span data-ttu-id="2ccf5-201">Por ejemplo, considere una aplicación lateral De Blazor-server con un componente que acepte y muestre una lista de elementos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-201">For example, consider a Blazor-server side app with a component that accepts and displays a list of items.</span></span> <span data-ttu-id="2ccf5-202">Si la aplicación Blazor no pone límites en el número de elementos permitidos o el número de elementos representados en el cliente, el procesamiento y la representación que consumen mucha memoria pueden dominar la memoria del servidor hasta el punto en que el rendimiento del servidor se ve afectado.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-202">If the Blazor app doesn't place limits on the number of items allowed or the number of items rendered back to the client, the memory-intensive processing and rendering may dominate the server's memory to the point where performance of the server suffers.</span></span> <span data-ttu-id="2ccf5-203">El servidor puede bloquearse o ralentizarse hasta el punto de que parece haberse bloqueado.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-203">The server may crash or slow to the point that it appears to have crashed.</span></span>

<span data-ttu-id="2ccf5-204">Considere el siguiente escenario para mantener y mostrar una lista de elementos que pertenecen a un escenario de agotamiento de memoria potencial en el servidor:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-204">Consider the following scenario for maintaining and displaying a list of items that pertain to a potential memory exhaustion scenario on the server:</span></span>

* <span data-ttu-id="2ccf5-205">Los elementos `List<MyItem>` de una propiedad o campo utilizan la memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-205">The items in a `List<MyItem>` property or field use the server's memory.</span></span> <span data-ttu-id="2ccf5-206">Si la aplicación permite que la lista de elementos crezca sin límites, existe el riesgo de que el servidor se quede sin memoria.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-206">If the app allows the list of items to grow unbounded, there's a risk of the server running out of memory.</span></span> <span data-ttu-id="2ccf5-207">Si se quede sin memoria, la sesión actual finaliza (se bloquea) y todas las sesiones simultáneas de esa instancia de servidor reciben una excepción de memoria insuficiente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-207">Running out of memory causes the current session to end (crash) and all of the concurrent sessions in that server instance receive an out-of-memory exception.</span></span> <span data-ttu-id="2ccf5-208">Para evitar que se produzca este escenario, la aplicación debe usar una estructura de datos que imponga un límite de elementos a los usuarios simultáneos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-208">To prevent this scenario from occurring, the app must use a data structure that imposes an item limit on concurrent users.</span></span>
* <span data-ttu-id="2ccf5-209">Si no se usa un esquema de paginación para la representación, el servidor usa memoria adicional para los objetos que no están visibles en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-209">If a paging scheme isn't used for rendering, the server uses additional memory for objects that aren't visible in the UI.</span></span> <span data-ttu-id="2ccf5-210">Sin un límite en el número de elementos, las demandas de memoria pueden agotar la memoria del servidor disponible.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-210">Without a limit on the number of items, memory demands may exhaust the available server memory.</span></span> <span data-ttu-id="2ccf5-211">Para evitar este escenario, utilice uno de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-211">To prevent this scenario, use one of the following approaches:</span></span>
  * <span data-ttu-id="2ccf5-212">Utilice listas paginadas al renderizar.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-212">Use paginated lists when rendering.</span></span>
  * <span data-ttu-id="2ccf5-213">Muestre solo los primeros 100 a 1.000 elementos y requiera que el usuario introduzca criterios de búsqueda para buscar elementos más allá de los elementos mostrados.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-213">Only display the first 100 to 1,000 items and require the user to enter search criteria to find items beyond the items displayed.</span></span>
  * <span data-ttu-id="2ccf5-214">Para un escenario de representación más avanzado, implemente listas o cuadrículas que admitan *la virtualización.*</span><span class="sxs-lookup"><span data-stu-id="2ccf5-214">For a more advanced rendering scenario, implement lists or grids that support *virtualization*.</span></span> <span data-ttu-id="2ccf5-215">Mediante la virtualización, las listas solo representan un subconjunto de elementos visibles actualmente para el usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-215">Using virtualization, lists only render a subset of items currently visible to the user.</span></span> <span data-ttu-id="2ccf5-216">Cuando el usuario interactúa con la barra de desplazamiento en la interfaz de usuario, el componente representa solo los elementos necesarios para la visualización.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-216">When the user interacts with the scrollbar in the UI, the component renders only those items required for display.</span></span> <span data-ttu-id="2ccf5-217">Los elementos que actualmente no son necesarios para la visualización se pueden mantener en almacenamiento secundario, que es el enfoque ideal.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-217">The items that aren't currently required for display can be held in secondary storage, which is the ideal approach.</span></span> <span data-ttu-id="2ccf5-218">Los elementos no mostrados también se pueden mantener en la memoria, lo que es menos ideal.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-218">Undisplayed items can also be held in memory, which is less ideal.</span></span>

<span data-ttu-id="2ccf5-219">Las aplicaciones de Blazor Server ofrecen un modelo de programación similar a otros marcos de interfaz de usuario para aplicaciones con estado, como WPF, Windows Forms o Blazor WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-219">Blazor Server apps offer a similar programming model to other UI frameworks for stateful apps, such as WPF, Windows Forms, or Blazor WebAssembly.</span></span> <span data-ttu-id="2ccf5-220">La principal diferencia es que en varios de los marcos de interfaz de usuario la memoria consumida por la aplicación pertenece al cliente y solo afecta a ese cliente individual.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-220">The main difference is that in several of the UI frameworks the memory consumed by the app belongs to the client and only affects that individual client.</span></span> <span data-ttu-id="2ccf5-221">Por ejemplo, una aplicación Blazor WebAssembly se ejecuta completamente en el cliente y solo usa recursos de memoria de cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-221">For example, a Blazor WebAssembly app runs entirely on the client and only uses client memory resources.</span></span> <span data-ttu-id="2ccf5-222">En el escenario De blazor Server, la memoria consumida por la aplicación pertenece al servidor y se comparte entre los clientes de la instancia del servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-222">In the Blazor Server scenario, the memory consumed by the app belongs to the server and is shared among clients on the server instance.</span></span>

<span data-ttu-id="2ccf5-223">Las demandas de memoria del lado del servidor son una consideración para todas las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-223">Server-side memory demands are a consideration for all Blazor Server apps.</span></span> <span data-ttu-id="2ccf5-224">Sin embargo, la mayoría de las aplicaciones web no tienen estado y la memoria utilizada durante el procesamiento de una solicitud se libera cuando se devuelve la respuesta.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-224">However, most web apps are stateless, and the memory used while processing a request is released when the response is returned.</span></span> <span data-ttu-id="2ccf5-225">Como recomendación general, no permita que los clientes asignen una cantidad de memoria sin enlazar como en cualquier otra aplicación del lado servidor que conserve las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-225">As a general recommendation, don't permit clients to allocate an unbound amount of memory as in any other server-side app that persists client connections.</span></span> <span data-ttu-id="2ccf5-226">La memoria consumida por una aplicación Blazor Server persiste durante más tiempo que una sola solicitud.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-226">The memory consumed by a Blazor Server app persists for a longer time than a single request.</span></span>

> [!NOTE]
> <span data-ttu-id="2ccf5-227">Durante el desarrollo, se puede utilizar un generador de perfiles o un seguimiento capturado para evaluar las demandas de memoria de los clientes.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-227">During development, a profiler can be used or a trace captured to assess memory demands of clients.</span></span> <span data-ttu-id="2ccf5-228">Un generador de perfiles o un seguimiento no capturará la memoria asignada a un cliente específico.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-228">A profiler or trace won't capture the memory allocated to a specific client.</span></span> <span data-ttu-id="2ccf5-229">Para capturar el uso de memoria de un cliente específico durante el desarrollo, capture un volcado y examine la demanda de memoria de todos los objetos rooteados en el circuito de un usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-229">To capture the memory use of a specific client during development, capture a dump and examine the memory demand of all the objects rooted at a user's circuit.</span></span>

### <a name="client-connections"></a><span data-ttu-id="2ccf5-230">Conexiones de cliente</span><span class="sxs-lookup"><span data-stu-id="2ccf5-230">Client connections</span></span>

<span data-ttu-id="2ccf5-231">El agotamiento de la conexión puede producirse cuando uno o más clientes abren demasiadas conexiones simultáneas con el servidor, lo que impide que otros clientes establezcan nuevas conexiones.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-231">Connection exhaustion can occur when one or more clients open too many concurrent connections to the server, preventing other clients from establishing new connections.</span></span>

<span data-ttu-id="2ccf5-232">Los clientes de Blazor establecen una sola conexión por sesión y mantienen la conexión abierta mientras la ventana del explorador esté abierta.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-232">Blazor clients establish a single connection per session and keep the connection open for as long as the browser window is open.</span></span> <span data-ttu-id="2ccf5-233">Las exigencias en el servidor de mantener todas las conexiones no es específica de las aplicaciones Blazor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-233">The demands on the server of maintaining all of the connections isn't specific to Blazor apps.</span></span> <span data-ttu-id="2ccf5-234">Dada la naturaleza persistente de las conexiones y la naturaleza con estado de las aplicaciones de Blazor Server, el agotamiento de la conexión es un mayor riesgo para la disponibilidad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-234">Given the persistent nature of the connections and the stateful nature of Blazor Server apps, connection exhaustion is a greater risk to availability of the app.</span></span>

<span data-ttu-id="2ccf5-235">De forma predeterminada, no hay límite en el número de conexiones por usuario para una aplicación de servidor de Blazor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-235">By default, there's no limit on the number of connections per user for a Blazor Server app.</span></span> <span data-ttu-id="2ccf5-236">Si la aplicación requiere un límite de conexión, tome uno o varios de los siguientes enfoques:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-236">If the app requires a connection limit, take one or more of the following approaches:</span></span>

* <span data-ttu-id="2ccf5-237">Requiere autenticación, que naturalmente limita la capacidad de los usuarios no autorizados para conectarse a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-237">Require authentication, which naturally limits the ability of unauthorized users to connect to the app.</span></span> <span data-ttu-id="2ccf5-238">Para que este escenario sea eficaz, se debe impedir que los usuarios aprovisionen nuevos usuarios a voluntad.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-238">For this scenario to be effective, users must be prevented from provisioning new users at will.</span></span>
* <span data-ttu-id="2ccf5-239">Limite el número de conexiones por usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-239">Limit the number of connections per user.</span></span> <span data-ttu-id="2ccf5-240">La limitación de conexiones se puede realizar mediante los siguientes enfoques.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-240">Limiting connections can be accomplished via the following approaches.</span></span> <span data-ttu-id="2ccf5-241">Tenga cuidado para permitir que los usuarios legítimos accedan a la aplicación (por ejemplo, cuando se establece un límite de conexión basado en la dirección IP del cliente).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-241">Exercise care to allow legitimate users to access the app (for example, when a connection limit is established based on the client's IP address).</span></span>
  * <span data-ttu-id="2ccf5-242">En el nivel de aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-242">At the app level:</span></span>
    * <span data-ttu-id="2ccf5-243">Extensibilidad de enrutamiento de punto final.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-243">Endpoint routing extensibility.</span></span>
    * <span data-ttu-id="2ccf5-244">Requerir autenticación para conectarse a la aplicación y realizar un seguimiento de las sesiones activas por usuario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-244">Require authentication to connect to the app and keep track of the active sessions per user.</span></span>
    * <span data-ttu-id="2ccf5-245">Rechazar nuevas sesiones al llegar a un límite.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-245">Reject new sessions upon reaching a limit.</span></span>
    * <span data-ttu-id="2ccf5-246">Proxy WebSocket se condatará a una aplicación mediante el uso de un proxy, como el [servicio Azure SignalR](/azure/azure-signalr/signalr-overview) que multiplexa las conexiones de los clientes a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-246">Proxy WebSocket connections to an app through the use of a proxy, such as the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) that multiplexes connections from clients to an app.</span></span> <span data-ttu-id="2ccf5-247">Esto proporciona una aplicación con mayor capacidad de conexión que un solo cliente puede establecer, lo que impide que un cliente agote las conexiones con el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-247">This provides an app with greater connection capacity than a single client can establish, preventing a client from exhausting the connections to the server.</span></span>
  * <span data-ttu-id="2ccf5-248">En el nivel de servidor: use un proxy/puerta de enlace delante de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-248">At the server level: Use a proxy/gateway in front of the app.</span></span> <span data-ttu-id="2ccf5-249">Por ejemplo, [Azure Front Door](/azure/frontdoor/front-door-overview) le permite definir, administrar y supervisar el enrutamiento global del tráfico web a una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-249">For example, [Azure Front Door](/azure/frontdoor/front-door-overview) enables you to define, manage, and monitor the global routing of web traffic to an app.</span></span>

## <a name="denial-of-service-dos-attacks"></a><span data-ttu-id="2ccf5-250">Ataques de denegación de servicio (DoS)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-250">Denial of service (DoS) attacks</span></span>

<span data-ttu-id="2ccf5-251">Los ataques de denegación de servicio (DoS) implican que un cliente agote uno o varios de sus recursos haciendo que la aplicación no esté disponible.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-251">Denial of service (DoS) attacks involve a client causing the server to exhaust one or more of its resources making the app unavailable.</span></span> <span data-ttu-id="2ccf5-252">Las aplicaciones de Blazor Server incluyen algunos límites predeterminados y dependen de otros ASP.NET límites Core y SignalR para protegerse contra ataques DoS:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-252">Blazor Server apps include some default limits and rely on other ASP.NET Core and SignalR limits to protect against DoS attacks:</span></span>

| <span data-ttu-id="2ccf5-253">Límite de aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="2ccf5-253">Blazor Server app limit</span></span>                            | <span data-ttu-id="2ccf5-254">Descripción</span><span class="sxs-lookup"><span data-stu-id="2ccf5-254">Description</span></span> | <span data-ttu-id="2ccf5-255">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="2ccf5-255">Default</span></span> |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | <span data-ttu-id="2ccf5-256">Número máximo de circuitos desconectados que un servidor determinado tiene en la memoria a la vez.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-256">Maximum number of disconnected circuits that a given server holds in memory at a time.</span></span> | <span data-ttu-id="2ccf5-257">100</span><span class="sxs-lookup"><span data-stu-id="2ccf5-257">100</span></span> |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | <span data-ttu-id="2ccf5-258">La cantidad máxima de tiempo que un circuito desconectado se mantiene en la memoria antes de ser derribado.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-258">Maximum amount of time a disconnected circuit is held in memory before being torn down.</span></span> | <span data-ttu-id="2ccf5-259">3 minutos</span><span class="sxs-lookup"><span data-stu-id="2ccf5-259">3 minutes</span></span> |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | <span data-ttu-id="2ccf5-260">Cantidad máxima de tiempo que el servidor espera antes de agotar el tiempo de espera de una invocación de función JavaScript asincrónica.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-260">Maximum amount of time the server waits before timing out an asynchronous JavaScript function invocation.</span></span> | <span data-ttu-id="2ccf5-261">1 minuto.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-261">1 minute</span></span> |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | <span data-ttu-id="2ccf5-262">Número máximo de lotes de renderización no reconocidos que el servidor mantiene en memoria por circuito en un momento dado para admitir la reconexión robusta.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-262">Maximum number of unacknowledged render batches the server keeps in memory per circuit at a given time to support robust reconnection.</span></span> <span data-ttu-id="2ccf5-263">Después de alcanzar el límite, el servidor deja de producir nuevos lotes de renderización hasta que el cliente haya reconocido uno o más lotes.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-263">After reaching the limit, the server stops producing new render batches until one or more batches have been acknowledged by the client.</span></span> | <span data-ttu-id="2ccf5-264">10</span><span class="sxs-lookup"><span data-stu-id="2ccf5-264">10</span></span> |


| <span data-ttu-id="2ccf5-265">Límite de SignalR y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ccf5-265">SignalR and ASP.NET Core limit</span></span>             | <span data-ttu-id="2ccf5-266">Descripción</span><span class="sxs-lookup"><span data-stu-id="2ccf5-266">Description</span></span> | <span data-ttu-id="2ccf5-267">Valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="2ccf5-267">Default</span></span> |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | <span data-ttu-id="2ccf5-268">Tamaño del mensaje para un mensaje individual.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-268">Message size for an individual message.</span></span> | <span data-ttu-id="2ccf5-269">32 KB</span><span class="sxs-lookup"><span data-stu-id="2ccf5-269">32 KB</span></span> |

## <a name="interactions-with-the-browser-client"></a><span data-ttu-id="2ccf5-270">Interacciones con el navegador (cliente)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-270">Interactions with the browser (client)</span></span>

<span data-ttu-id="2ccf5-271">Un cliente interactúa con el servidor a través de la distribución de eventos de interoperabilidad JS y la finalización de la representación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-271">A client interacts with the server through JS interop event dispatching and render completion.</span></span> <span data-ttu-id="2ccf5-272">La comunicación de interoperabilidad de JS va en ambos sentidos entre JavaScript y .NET:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-272">JS interop communication goes both ways between JavaScript and .NET:</span></span>

* <span data-ttu-id="2ccf5-273">Los eventos del explorador se distribuyen desde el cliente al servidor de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-273">Browser events are dispatched from the client to the server in an asynchronous fashion.</span></span>
* <span data-ttu-id="2ccf5-274">El servidor responde de forma asincrónica rerepresentando la interfaz de usuario según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-274">The server responds asynchronously rerendering the UI as necessary.</span></span>

### <a name="javascript-functions-invoked-from-net"></a><span data-ttu-id="2ccf5-275">Funciones JavaScript invocadas desde .NET</span><span class="sxs-lookup"><span data-stu-id="2ccf5-275">JavaScript functions invoked from .NET</span></span>

<span data-ttu-id="2ccf5-276">Para llamadas desde métodos .NET a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-276">For calls from .NET methods to JavaScript:</span></span>

* <span data-ttu-id="2ccf5-277">Todas las invocaciones tienen un tiempo de espera <xref:System.OperationCanceledException> configurable después del cual fallan, devolviendo un al llamador.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-277">All invocations have a configurable timeout after which they fail, returning a <xref:System.OperationCanceledException> to the caller.</span></span>
  * <span data-ttu-id="2ccf5-278">Hay un tiempo de espera`CircuitOptions.JSInteropDefaultCallTimeout`predeterminado para las llamadas ( ) de un minuto.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-278">There's a default timeout for the calls (`CircuitOptions.JSInteropDefaultCallTimeout`) of one minute.</span></span> <span data-ttu-id="2ccf5-279">Para configurar este <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>límite, consulte .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-279">To configure this limit, see <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>.</span></span>
  * <span data-ttu-id="2ccf5-280">Se puede proporcionar un token de cancelación para controlar la cancelación por llamada.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-280">A cancellation token can be provided to control the cancellation on a per-call basis.</span></span> <span data-ttu-id="2ccf5-281">Confíe en el tiempo de espera de llamada predeterminado siempre que sea posible y de cualquier llamada al cliente si se proporciona un token de cancelación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-281">Rely on the default call timeout where possible and time-bound any call to the client if a cancellation token is provided.</span></span>
* <span data-ttu-id="2ccf5-282">No se puede confiar en el resultado de una llamada de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-282">The result of a JavaScript call can't be trusted.</span></span> <span data-ttu-id="2ccf5-283">El Blazor cliente de aplicación que se ejecuta en el explorador busca la función JavaScript que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-283">The Blazor app client running in the browser searches for the JavaScript function to invoke.</span></span> <span data-ttu-id="2ccf5-284">Se invoca la función y se genera el resultado o un error.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-284">The function is invoked, and either the result or an error is produced.</span></span> <span data-ttu-id="2ccf5-285">Un cliente malintencionado puede intentar:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-285">A malicious client can attempt to:</span></span>
  * <span data-ttu-id="2ccf5-286">Causa un problema en la aplicación devolviendo un error de la función JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-286">Cause an issue in the app by returning an error from the JavaScript function.</span></span>
  * <span data-ttu-id="2ccf5-287">Inducir un comportamiento no deseado en el servidor devolviendo un resultado inesperado de la función JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-287">Induce an unintended behavior on the server by returning an unexpected result from the JavaScript function.</span></span>

<span data-ttu-id="2ccf5-288">Tome las siguientes precauciones para protegerse contra los escenarios anteriores:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-288">Take the following precautions to guard against the preceding scenarios:</span></span>

* <span data-ttu-id="2ccf5-289">Ajuste las llamadas de interoperabilidad de JS dentro de instrucciones [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) para tener en cuenta los errores que pueden producirse durante las invocaciones.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-289">Wrap JS interop calls within [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements to account for errors that might occur during the invocations.</span></span> <span data-ttu-id="2ccf5-290">Para obtener más información, vea <xref:blazor/handle-errors#javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-290">For more information, see <xref:blazor/handle-errors#javascript-interop>.</span></span>
* <span data-ttu-id="2ccf5-291">Validar los datos devueltos por invocaciones de interoperabilidad de JS, incluidos los mensajes de error, antes de realizar cualquier acción.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-291">Validate data returned from JS interop invocations, including error messages, before taking any action.</span></span>

### <a name="net-methods-invoked-from-the-browser"></a><span data-ttu-id="2ccf5-292">Métodos .NET invocados desde el explorador</span><span class="sxs-lookup"><span data-stu-id="2ccf5-292">.NET methods invoked from the browser</span></span>

<span data-ttu-id="2ccf5-293">No confíe en las llamadas de JavaScript a métodos .NET.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-293">Don't trust calls from JavaScript to .NET methods.</span></span> <span data-ttu-id="2ccf5-294">Cuando un método .NET se expone a JavaScript, tenga en cuenta cómo se invoca el método .NET:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-294">When a .NET method is exposed to JavaScript, consider how the .NET method is invoked:</span></span>

* <span data-ttu-id="2ccf5-295">Tratar cualquier método .NET expuesto a JavaScript como lo haría con un punto de conexión público a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-295">Treat any .NET method exposed to JavaScript as you would a public endpoint to the app.</span></span>
  * <span data-ttu-id="2ccf5-296">Validar entrada.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-296">Validate input.</span></span>
    * <span data-ttu-id="2ccf5-297">Asegúrese de que los valores están dentro de los rangos esperados.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-297">Ensure that values are within expected ranges.</span></span>
    * <span data-ttu-id="2ccf5-298">Asegúrese de que el usuario tiene permiso para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-298">Ensure that the user has permission to perform the action requested.</span></span>
  * <span data-ttu-id="2ccf5-299">No asigne una cantidad excesiva de recursos como parte de la invocación del método .NET.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-299">Don't allocate an excessive quantity of resources as part of the .NET method invocation.</span></span> <span data-ttu-id="2ccf5-300">Por ejemplo, realice comprobaciones y coloque límites en el uso de CPU y memoria.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-300">For example, perform checks and place limits on CPU and memory use.</span></span>
  * <span data-ttu-id="2ccf5-301">Tenga en cuenta que los métodos estáticos y de instancia se pueden exponer a clientes JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-301">Take into account that static and instance methods can be exposed to JavaScript clients.</span></span> <span data-ttu-id="2ccf5-302">Evite compartir el estado entre sesiones a menos que el diseño llame al estado de uso compartido con las restricciones adecuadas.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-302">Avoid sharing state across sessions unless the design calls for sharing state with appropriate constraints.</span></span>
    * <span data-ttu-id="2ccf5-303">Por ejemplo, `DotNetReference` los métodos expuestos a través de objetos creados originalmente a través de la inserción de dependencias (DI), los objetos deben registrarse como objetos con ámbito.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-303">For instance methods exposed through `DotNetReference` objects that are originally created through dependency injection (DI), the objects should be registered as scoped objects.</span></span> <span data-ttu-id="2ccf5-304">Esto se aplica a Blazor cualquier servicio di que use la aplicación De servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-304">This applies to any DI service that the Blazor Server app uses.</span></span>
    * <span data-ttu-id="2ccf5-305">Para los métodos estáticos, evite establecer un estado que no se pueda limitar al cliente a menos que la aplicación comparta explícitamente el estado por diseño entre todos los usuarios de una instancia de servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-305">For static methods, avoid establishing state that can't be scoped to the client unless the app is explicitly sharing state by-design across all users on a server instance.</span></span>
  * <span data-ttu-id="2ccf5-306">Evite pasar datos proporcionados por el usuario en parámetros a llamadas JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-306">Avoid passing user-supplied data in parameters to JavaScript calls.</span></span> <span data-ttu-id="2ccf5-307">Si es absolutamente necesario pasar datos en parámetros, asegúrese de que el código JavaScript controla el paso de los datos sin introducir vulnerabilidades de secuencias de comandos entre sitios [(XSS).](#cross-site-scripting-xss)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-307">If passing data in parameters is absolutely required, ensure that the JavaScript code handles passing the data without introducing [Cross-site scripting (XSS)](#cross-site-scripting-xss) vulnerabilities.</span></span> <span data-ttu-id="2ccf5-308">Por ejemplo, no escriba datos proporcionados por el usuario en `innerHTML` el modelo de objetos de documento (DOM) estableciendo la propiedad de un elemento.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-308">For example, don't write user-supplied data to the Document Object Model (DOM) by setting the `innerHTML` property of an element.</span></span> <span data-ttu-id="2ccf5-309">Considere la posibilidad de usar `eval` Content Security Policy [(CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) para deshabilitar y otros primitivos de JavaScript no seguros.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-309">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to disable `eval` and other unsafe JavaScript primitives.</span></span>
* <span data-ttu-id="2ccf5-310">Evite implementar la distribución personalizada de invocaciones de .NET además de la implementación de distribución del marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-310">Avoid implementing custom dispatching of .NET invocations on top of the framework's dispatching implementation.</span></span> <span data-ttu-id="2ccf5-311">La exposición de métodos .NET al explorador Blazor es un escenario avanzado, no se recomienda para el desarrollo general.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-311">Exposing .NET methods to the browser is an advanced scenario, not recommended for general Blazor development.</span></span>

### <a name="events"></a><span data-ttu-id="2ccf5-312">Events</span><span class="sxs-lookup"><span data-stu-id="2ccf5-312">Events</span></span>

<span data-ttu-id="2ccf5-313">Los eventos proporcionan un Blazor punto de entrada a una aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-313">Events provide an entry point to a Blazor Server app.</span></span> <span data-ttu-id="2ccf5-314">Las mismas reglas para proteger los puntos de Blazor conexión en aplicaciones web se aplican al control de eventos en las aplicaciones de servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-314">The same rules for safeguarding endpoints in web apps apply to event handling in Blazor Server apps.</span></span> <span data-ttu-id="2ccf5-315">Un cliente malintencionado puede enviar los datos que desea enviar como carga útil para un evento.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-315">A malicious client can send any data it wishes to send as the payload for an event.</span></span>

<span data-ttu-id="2ccf5-316">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-316">For example:</span></span>

* <span data-ttu-id="2ccf5-317">Un evento de `<select>` cambio para un podría enviar un valor que no está dentro de las opciones que la aplicación presentó al cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-317">A change event for a `<select>` could send a value that isn't within the options that the app presented to the client.</span></span>
* <span data-ttu-id="2ccf5-318">Un `<input>` podría enviar cualquier dato de texto al servidor, omitiendo la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-318">An `<input>` could send any text data to the server, bypassing client-side validation.</span></span>

<span data-ttu-id="2ccf5-319">La aplicación debe validar los datos de cualquier evento que controle la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-319">The app must validate the data for any event that the app handles.</span></span> <span data-ttu-id="2ccf5-320">El Blazor marco de trabajo forma que los [componentes](xref:blazor/forms-validation) realicen validaciones básicas.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-320">The Blazor framework [forms components](xref:blazor/forms-validation) perform basic validations.</span></span> <span data-ttu-id="2ccf5-321">Si la aplicación usa componentes de formularios personalizados, se debe escribir código personalizado para validar los datos de eventos según corresponda.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-321">If the app uses custom forms components, custom code must be written to validate event data as appropriate.</span></span>

Blazor<span data-ttu-id="2ccf5-322">Los eventos de servidor son asincrónicos, por lo que se pueden distribuir varios eventos al servidor antes de que la aplicación tenga tiempo para reaccionar produciendo un nuevo procesamiento.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-322"> Server events are asynchronous, so multiple events can be dispatched to the server before the app has time to react by producing a new render.</span></span> <span data-ttu-id="2ccf5-323">Esto tiene algunas implicaciones de seguridad a tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-323">This has some security implications to consider.</span></span> <span data-ttu-id="2ccf5-324">Limitar las acciones de cliente en la aplicación debe realizarse dentro de los controladores de eventos y no depender del estado de vista representado actual.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-324">Limiting client actions in the app must be performed inside event handlers and not depend on the current rendered view state.</span></span>

<span data-ttu-id="2ccf5-325">Considere un componente de contador que debe permitir a un usuario incrementar un contador un máximo de tres veces.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-325">Consider a counter component that should allow a user to increment a counter a maximum of three times.</span></span> <span data-ttu-id="2ccf5-326">El botón para incrementar el contador se `count`basa condicionalmente en el valor de:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-326">The button to increment the counter is conditionally based on the value of `count`:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        count++;
    }
}
```

<span data-ttu-id="2ccf5-327">Un cliente puede distribuir uno o varios eventos de incremento antes de que el marco de trabajo genere una nueva representación de este componente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-327">A client can dispatch one or more increment events before the framework produces a new render of this component.</span></span> <span data-ttu-id="2ccf5-328">El resultado es `count` que el usuario puede incrementarse más de *tres veces* porque la interfaz de usuario no quita el botón lo suficientemente rápido.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-328">The result is that the `count` can be incremented *over three times* by the user because the button isn't removed by the UI quickly enough.</span></span> <span data-ttu-id="2ccf5-329">En el ejemplo siguiente se `count` muestra la forma correcta de alcanzar el límite de tres incrementos:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-329">The correct way to achieve the limit of three `count` increments is shown in the following example:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        if (count < 3)
        {
            count++;
        }
    }
}
```

<span data-ttu-id="2ccf5-330">Al agregar `if (count < 3) { ... }` la comprobación dentro del `count` controlador, la decisión de incrementar se basa en el estado actual de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-330">By adding the `if (count < 3) { ... }` check inside the handler, the decision to increment `count` is based on the current app state.</span></span> <span data-ttu-id="2ccf5-331">La decisión no se basa en el estado de la interfaz de usuario como estaba en el ejemplo anterior, que podría estar temporalmente obsoleto.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-331">The decision isn't based on the state of the UI as it was in the previous example, which might be temporarily stale.</span></span>

### <a name="guard-against-multiple-dispatches"></a><span data-ttu-id="2ccf5-332">Protegerse contra múltiples despachos</span><span class="sxs-lookup"><span data-stu-id="2ccf5-332">Guard against multiple dispatches</span></span>

<span data-ttu-id="2ccf5-333">Si una devolución de llamada de evento invoca una operación de larga ejecución de forma asincrónica, como la obtención de datos de un servicio externo o una base de datos, considere la posibilidad de usar una protección.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-333">If an event callback invokes a long running operation asynchronously, such as fetching data from an external service or database, consider using a guard.</span></span> <span data-ttu-id="2ccf5-334">La protección puede impedir que el usuario haga cola varias operaciones mientras la operación está en curso con comentarios visuales.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-334">The guard can prevent the user from queueing up multiple operations while the operation is in progress with visual feedback.</span></span> <span data-ttu-id="2ccf5-335">El siguiente código `isLoading` `true` de `GetForecastAsync` componente se establece en while obtiene datos del servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-335">The following component code sets `isLoading` to `true` while `GetForecastAsync` obtains data from the server.</span></span> <span data-ttu-id="2ccf5-336">`isLoading` Mientras `true`está , el botón está deshabilitado en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-336">While `isLoading` is `true`, the button is disabled in the UI:</span></span>

```razor
@page "/fetchdata"
@using BlazorServerSample.Data
@inject WeatherForecastService ForecastService

<button disabled="@isLoading" @onclick="UpdateForecasts">Update</button>

@code {
    private bool isLoading;
    private WeatherForecast[] forecasts;

    private async Task UpdateForecasts()
    {
        if (!isLoading)
        {
            isLoading = true;
            forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
            isLoading = false;
        }
    }
}
```

<span data-ttu-id="2ccf5-337">El patrón de protección demostrado en el ejemplo anterior funciona `async` - `await` si la operación en segundo plano se ejecuta de forma asincrónica con el patrón.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-337">The guard pattern demonstrated in the preceding example works if the background operation is executed asynchronously with the `async`-`await` pattern.</span></span>

### <a name="cancel-early-and-avoid-use-after-dispose"></a><span data-ttu-id="2ccf5-338">Cancele a tiempo y evite el uso después de desechar</span><span class="sxs-lookup"><span data-stu-id="2ccf5-338">Cancel early and avoid use-after-dispose</span></span>

<span data-ttu-id="2ccf5-339">Además de usar un protector como se describe en <xref:System.Threading.CancellationToken> la sección Proteger contra varios [envíos,](#guard-against-multiple-dispatches) considere la posibilidad de usar a para cancelar operaciones de ejecución prolongada cuando se elimina el componente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-339">In addition to using a guard as described in the [Guard against multiple dispatches](#guard-against-multiple-dispatches) section, consider using a <xref:System.Threading.CancellationToken> to cancel long-running operations when the component is disposed.</span></span> <span data-ttu-id="2ccf5-340">Este enfoque tiene la ventaja añadida de evitar el *uso después de la eliminación* en los componentes:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-340">This approach has the added benefit of avoiding *use-after-dispose* in components:</span></span>

```razor
@implements IDisposable

...

@code {
    private readonly CancellationTokenSource TokenSource = 
        new CancellationTokenSource();

    private async Task UpdateForecasts()
    {
        ...

        forecasts = await ForecastService.GetForecastAsync(DateTime.Now, 
            TokenSource.Token);

        if (TokenSource.Token.IsCancellationRequested)
        {
           return;
        }

        ...
    }

    public void Dispose()
    {
        TokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a><span data-ttu-id="2ccf5-341">Evite eventos que produzcan grandes cantidades de datos</span><span class="sxs-lookup"><span data-stu-id="2ccf5-341">Avoid events that produce large amounts of data</span></span>

<span data-ttu-id="2ccf5-342">Algunos eventos DOM, `oninput` `onscroll`como o , pueden producir una gran cantidad de datos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-342">Some DOM events, such as `oninput` or `onscroll`, can produce a large amount of data.</span></span> <span data-ttu-id="2ccf5-343">Evite usar estos Blazor eventos en aplicaciones de servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-343">Avoid using these events in Blazor server apps.</span></span>

## <a name="additional-security-guidance"></a><span data-ttu-id="2ccf5-344">Orientación de seguridad adicional</span><span class="sxs-lookup"><span data-stu-id="2ccf5-344">Additional security guidance</span></span>

<span data-ttu-id="2ccf5-345">Las instrucciones para proteger ASP.NET aplicaciones Blazor principales se aplican a las aplicaciones de servidor y se tratan en las secciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-345">The guidance for securing ASP.NET Core apps apply to Blazor Server apps and are covered in the following sections:</span></span>

* [<span data-ttu-id="2ccf5-346">Registro y datos confidenciales</span><span class="sxs-lookup"><span data-stu-id="2ccf5-346">Logging and sensitive data</span></span>](#logging-and-sensitive-data)
* [<span data-ttu-id="2ccf5-347">Proteger la información en tránsito con HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ccf5-347">Protect information in transit with HTTPS</span></span>](#protect-information-in-transit-with-https)
* <span data-ttu-id="2ccf5-348">[Scripting entre sitios (XSS)](#cross-site-scripting-xss))</span><span class="sxs-lookup"><span data-stu-id="2ccf5-348">[Cross-site scripting (XSS)](#cross-site-scripting-xss))</span></span>
* [<span data-ttu-id="2ccf5-349">Protección entre orígenes</span><span class="sxs-lookup"><span data-stu-id="2ccf5-349">Cross-origin protection</span></span>](#cross-origin-protection)
* [<span data-ttu-id="2ccf5-350">Click-jacking</span><span class="sxs-lookup"><span data-stu-id="2ccf5-350">Click-jacking</span></span>](#click-jacking)
* [<span data-ttu-id="2ccf5-351">Redirecciones abiertas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-351">Open redirects</span></span>](#open-redirects)

### <a name="logging-and-sensitive-data"></a><span data-ttu-id="2ccf5-352">Registro y datos confidenciales</span><span class="sxs-lookup"><span data-stu-id="2ccf5-352">Logging and sensitive data</span></span>

<span data-ttu-id="2ccf5-353">Las interacciones de interoperabilidad de JS entre el <xref:Microsoft.Extensions.Logging.ILogger> cliente y el servidor se registran en los registros del servidor con instancias.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-353">JS interop interactions between the client and server are recorded in the server's logs with <xref:Microsoft.Extensions.Logging.ILogger> instances.</span></span> Blazor<span data-ttu-id="2ccf5-354">evita el registro de información confidencial, como eventos reales o entradas y salidas de interoperabilidad JS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-354"> avoids logging sensitive information, such as actual events or JS interop inputs and outputs.</span></span>

<span data-ttu-id="2ccf5-355">Cuando se produce un error en el servidor, el marco de trabajo notifica al cliente y derriba la sesión.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-355">When an error occurs on the server, the framework notifies the client and tears down the session.</span></span> <span data-ttu-id="2ccf5-356">De forma predeterminada, el cliente recibe un mensaje de error genérico que se puede ver en las herramientas de desarrollador del explorador.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-356">By default, the client receives a generic error message that can be seen in the browser's developer tools.</span></span>

<span data-ttu-id="2ccf5-357">El error del lado cliente no incluye la pila de llamadas y no proporciona detalles sobre la causa del error, pero los registros del servidor contienen dicha información.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-357">The client-side error doesn't include the callstack and doesn't provide detail on the cause of the error, but server logs do contain such information.</span></span> <span data-ttu-id="2ccf5-358">Para fines de desarrollo, la información de error confidencial se puede poner a disposición del cliente habilitando errores detallados.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-358">For development purposes, sensitive error information can be made available to the client by enabling detailed errors.</span></span>

<span data-ttu-id="2ccf5-359">Habilite errores detallados con:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-359">Enable detailed errors with:</span></span>

* <span data-ttu-id="2ccf5-360">`CircuitOptions.DetailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-360">`CircuitOptions.DetailedErrors`.</span></span>
* <span data-ttu-id="2ccf5-361">`DetailedErrors`clave de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-361">`DetailedErrors` configuration key.</span></span> <span data-ttu-id="2ccf5-362">Por ejemplo, `ASPNETCORE_DETAILEDERRORS` establezca la variable `true`de entorno en un valor de .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-362">For example, set the `ASPNETCORE_DETAILEDERRORS` environment variable to a value of `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="2ccf5-363">La exposición de información de error a los clientes en Internet es un riesgo de seguridad que siempre debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-363">Exposing error information to clients on the Internet is a security risk that should always be avoided.</span></span>

### <a name="protect-information-in-transit-with-https"></a><span data-ttu-id="2ccf5-364">Proteger la información en tránsito con HTTPS</span><span class="sxs-lookup"><span data-stu-id="2ccf5-364">Protect information in transit with HTTPS</span></span>

Blazor<span data-ttu-id="2ccf5-365">El SignalR servidor se utiliza para la comunicación entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-365"> Server uses SignalR for communication between the client and the server.</span></span> Blazor<span data-ttu-id="2ccf5-366">Server normalmente usa SignalR el transporte que negocia, que suele ser WebSockets.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-366"> Server normally uses the transport that SignalR negotiates, which is typically WebSockets.</span></span>

Blazor<span data-ttu-id="2ccf5-367">El servidor no garantiza la integridad y confidencialidad de los datos enviados entre el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-367"> Server doesn't ensure the integrity and confidentiality of the data sent between the server and the client.</span></span> <span data-ttu-id="2ccf5-368">Utilice siempre HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-368">Always use HTTPS.</span></span>

### <a name="cross-site-scripting-xss"></a><span data-ttu-id="2ccf5-369">Scripting entre sitios (XSS)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-369">Cross-site scripting (XSS)</span></span>

<span data-ttu-id="2ccf5-370">El scripting entre sitios (XSS) permite a una parte no autorizada ejecutar lógica arbitraria en el contexto del explorador.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-370">Cross-site scripting (XSS) allows an unauthorized party to execute arbitrary logic in the context of the browser.</span></span> <span data-ttu-id="2ccf5-371">Una aplicación comprometida podría ejecutar código arbitrario en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-371">A compromised app could potentially run arbitrary code on the client.</span></span> <span data-ttu-id="2ccf5-372">La vulnerabilidad podría utilizarse para realizar potencialmente una serie de acciones maliciosas contra el servidor:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-372">The vulnerability could be used to potentially perform a number of malicious actions against the server:</span></span>

* <span data-ttu-id="2ccf5-373">Enviar eventos falsos/no válidos al servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-373">Dispatch fake/invalid events to the server.</span></span>
* <span data-ttu-id="2ccf5-374">Finalizaciones de renderización no válidas o con errores de envío.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-374">Dispatch fail/invalid render completions.</span></span>
* <span data-ttu-id="2ccf5-375">Evite enviar las terminaciones de renderización.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-375">Avoid dispatching render completions.</span></span>
* <span data-ttu-id="2ccf5-376">Distribuir llamadas de interoperabilidad de JavaScript a .NET.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-376">Dispatch interop calls from JavaScript to .NET.</span></span>
* <span data-ttu-id="2ccf5-377">Modifique la respuesta de las llamadas de interoperabilidad de .NET a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-377">Modify the response of interop calls from .NET to JavaScript.</span></span>
* <span data-ttu-id="2ccf5-378">Evite distribuir resultados de interoperabilidad de .NET a JS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-378">Avoid dispatching .NET to JS interop results.</span></span>

<span data-ttu-id="2ccf5-379">El Blazor marco de trabajo del servidor toma medidas para protegerse contra algunas de las amenazas anteriores:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-379">The Blazor Server framework takes steps to protect against some of the preceding threats:</span></span>

* <span data-ttu-id="2ccf5-380">Deja de producir nuevas actualizaciones de la interfaz de usuario si el cliente no reconoce lotes de representación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-380">Stops producing new UI updates if the client isn't acknowledging render batches.</span></span> <span data-ttu-id="2ccf5-381">Configurado `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`con .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-381">Configured with `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`.</span></span>
* <span data-ttu-id="2ccf5-382">Agota el tiempo de espera de cualquier llamada de .NET a JavaScript después de un minuto sin recibir una respuesta del cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-382">Times out any .NET to JavaScript call after one minute without receiving a response from the client.</span></span> <span data-ttu-id="2ccf5-383">Configurado `CircuitOptions.JSInteropDefaultCallTimeout`con .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-383">Configured with `CircuitOptions.JSInteropDefaultCallTimeout`.</span></span>
* <span data-ttu-id="2ccf5-384">Realiza la validación básica en todas las entradas procedentes del navegador durante la interoperabilidad de JS:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-384">Performs basic validation on all input coming from the browser during JS interop:</span></span>
  * <span data-ttu-id="2ccf5-385">Las referencias de .NET son válidas y del tipo esperado por el método .NET.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-385">.NET references are valid and of the type expected by the .NET method.</span></span>
  * <span data-ttu-id="2ccf5-386">Los datos no están mal formados.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-386">The data isn't malformed.</span></span>
  * <span data-ttu-id="2ccf5-387">El número correcto de argumentos para el método está presente en la carga.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-387">The correct number of arguments for the method are present in the payload.</span></span>
  * <span data-ttu-id="2ccf5-388">Los argumentos o el resultado se pueden deserializar correctamente antes de invocar el método.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-388">The arguments or result can be deserialized correctly before invoking the method.</span></span>
* <span data-ttu-id="2ccf5-389">Realiza la validación básica en todas las entradas procedentes del explorador de eventos distribuidos:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-389">Performs basic validation in all input coming from the browser from dispatched events:</span></span>
  * <span data-ttu-id="2ccf5-390">El evento tiene un tipo válido.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-390">The event has a valid type.</span></span>
  * <span data-ttu-id="2ccf5-391">Los datos del evento se pueden deserializar.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-391">The data for the event can be deserialized.</span></span>
  * <span data-ttu-id="2ccf5-392">Hay un controlador de eventos asociado al evento.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-392">There's an event handler associated with the event.</span></span>

<span data-ttu-id="2ccf5-393">Además de las protecciones que implementa el marco de trabajo, el desarrollador debe codificar la aplicación para protegerse contra las amenazas y tomar las medidas adecuadas:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-393">In addition to the safeguards that the framework implements, the app must be coded by the developer to safeguard against threats and take appropriate actions:</span></span>

* <span data-ttu-id="2ccf5-394">Valide siempre los datos al controlar eventos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-394">Always validate data when handling events.</span></span>
* <span data-ttu-id="2ccf5-395">Tome las medidas apropiadas al recibir datos no válidos:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-395">Take appropriate action upon receiving invalid data:</span></span>
  * <span data-ttu-id="2ccf5-396">Ignore los datos y vuelva.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-396">Ignore the data and return.</span></span> <span data-ttu-id="2ccf5-397">Esto permite que la aplicación continúe procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-397">This allows the app to continue processing requests.</span></span>
  * <span data-ttu-id="2ccf5-398">Si la aplicación determina que la entrada es ilegítima y no puede ser producida por el cliente legítimo, inicie una excepción.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-398">If the app determines that the input is illegitimate and couldn't be produced by legitimate client, throw an exception.</span></span> <span data-ttu-id="2ccf5-399">Lanzar una excepción derriba el circuito y finaliza la sesión.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-399">Throwing an exception tears down the circuit and ends the session.</span></span>
* <span data-ttu-id="2ccf5-400">No confíe en el mensaje de error proporcionado por las terminaciones de lote de procesamiento incluidas en los registros.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-400">Don't trust the error message provided by render batch completions included in the logs.</span></span> <span data-ttu-id="2ccf5-401">El cliente proporciona el error y, por lo general, no se puede confiar en el los que se puede confiar, ya que el cliente podría verse comprometido.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-401">The error is provided by the client and can't generally be trusted, as the client might be compromised.</span></span>
* <span data-ttu-id="2ccf5-402">No confíe en la entrada de las llamadas de interoperabilidad de JS en cualquier dirección entre los métodos JavaScript y .NET.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-402">Don't trust the input on JS interop calls in either direction between JavaScript and .NET methods.</span></span>
* <span data-ttu-id="2ccf5-403">La aplicación es responsable de validar que el contenido de los argumentos y los resultados son válidos, incluso si los argumentos o resultados se deserializan correctamente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-403">The app is responsible for validating that the content of arguments and results are valid, even if the arguments or results are correctly deserialized.</span></span>

<span data-ttu-id="2ccf5-404">Para que exista una vulnerabilidad XSS, la aplicación debe incorporar la entrada del usuario en la página representada.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-404">For a XSS vulnerability to exist, the app must incorporate user input in the rendered page.</span></span> Blazor<span data-ttu-id="2ccf5-405">Los componentes del servidor ejecutan un paso en tiempo de compilación en el que el marcado de un archivo *.razor* se transforma en lógica de procedimiento de C.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-405"> Server components execute a compile-time step where the markup in a *.razor* file is transformed into procedural C# logic.</span></span> <span data-ttu-id="2ccf5-406">En tiempo de ejecución, la lógica de C- crea un árbol de *representación* que describe los elementos, el texto y los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-406">At runtime, the C# logic builds a *render tree* describing the elements, text, and child components.</span></span> <span data-ttu-id="2ccf5-407">Esto se aplica al DOM del navegador a través de una secuencia de instrucciones de JavaScript (o se serializa en HTML en el caso de preprocesamiento):</span><span class="sxs-lookup"><span data-stu-id="2ccf5-407">This is applied to the browser's DOM via a sequence of JavaScript instructions (or is serialized to HTML in the case of prerendering):</span></span>

* <span data-ttu-id="2ccf5-408">La entrada del usuario representada a `@someStringValue`través de la sintaxis normal de Razor (por ejemplo, ) no expone una vulnerabilidad XSS porque la sintaxis de Razor se agrega al DOM a través de comandos que solo pueden escribir texto.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-408">User input rendered via normal Razor syntax (for example, `@someStringValue`) doesn't expose a XSS vulnerability because the Razor syntax is added to the DOM via commands that can only write text.</span></span> <span data-ttu-id="2ccf5-409">Incluso si el valor incluye marcado HTML, el valor se muestra como texto estático.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-409">Even if the value includes HTML markup, the value is displayed as static text.</span></span> <span data-ttu-id="2ccf5-410">Al prerenderizar, la salida está codificada en HTML, que también muestra el contenido como texto estático.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-410">When prerendering, the output is HTML-encoded, which also displays the content as static text.</span></span>
* <span data-ttu-id="2ccf5-411">Las etiquetas de script no están permitidas y no deben incluirse en el árbol de procesamiento de componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-411">Script tags aren't allowed and shouldn't be included in the app's component render tree.</span></span> <span data-ttu-id="2ccf5-412">Si se incluye una etiqueta de script en el marcado de un componente, se genera un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-412">If a script tag is included in a component's markup, a compile-time error is generated.</span></span>
* <span data-ttu-id="2ccf5-413">Los autores de componentes pueden crear componentes en C- sin usar Razor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-413">Component authors can author components in C# without using Razor.</span></span> <span data-ttu-id="2ccf5-414">El autor del componente es responsable de utilizar las API correctas al emitir la salida.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-414">The component author is responsible for using the correct APIs when emitting output.</span></span> <span data-ttu-id="2ccf5-415">Por ejemplo, `builder.AddContent(0, someUserSuppliedString)` use y no , *ya* `builder.AddMarkupContent(0, someUserSuppliedString)`que este último podría crear una vulnerabilidad XSS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-415">For example, use `builder.AddContent(0, someUserSuppliedString)` and *not* `builder.AddMarkupContent(0, someUserSuppliedString)`, as the latter could create a XSS vulnerability.</span></span>

<span data-ttu-id="2ccf5-416">Como parte de la protección contra ataques XSS, considere la posibilidad de implementar mitigaciones XSS, como la directiva de seguridad de contenido [(CSP).](https://developer.mozilla.org/docs/Web/HTTP/CSP)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-416">As part of protecting against XSS attacks, consider implementing XSS mitigations, such as [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP).</span></span>

<span data-ttu-id="2ccf5-417">Para obtener más información, vea <xref:security/cross-site-scripting>.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-417">For more information, see <xref:security/cross-site-scripting>.</span></span>

### <a name="cross-origin-protection"></a><span data-ttu-id="2ccf5-418">Protección entre orígenes</span><span class="sxs-lookup"><span data-stu-id="2ccf5-418">Cross-origin protection</span></span>

<span data-ttu-id="2ccf5-419">Los ataques entre orígenes implican a un cliente de un origen diferente que realiza una acción contra el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-419">Cross-origin attacks involve a client from a different origin performing an action against the server.</span></span> <span data-ttu-id="2ccf5-420">La acción maliciosa suele ser una solicitud GET o un formulario POST (Cross-Site Request Forgery, CSRF), pero también es posible abrir un WebSocket malintencionado.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-420">The malicious action is typically a GET request or a form POST (Cross-Site Request Forgery, CSRF), but opening a malicious WebSocket is also possible.</span></span> Blazor<span data-ttu-id="2ccf5-421">Las aplicaciones de servidor ofrecen [las mismas garantías que cualquier otra SignalR aplicación que utilice el protocolo hub ofrece:](xref:signalr/security)</span><span class="sxs-lookup"><span data-stu-id="2ccf5-421"> Server apps offer [the same guarantees that any other SignalR app using the hub protocol offer](xref:signalr/security):</span></span>

* Blazor<span data-ttu-id="2ccf5-422">Se puede acceder a las aplicaciones de servidor entre orígenes a menos que se tomen medidas adicionales para evitarlo.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-422"> Server apps can be accessed cross-origin unless additional measures are taken to prevent it.</span></span> <span data-ttu-id="2ccf5-423">Para deshabilitar el acceso entre orígenes, deshabilite CORS en el punto de `DisableCorsAttribute` conexión Blazor agregando el middleware CORS a la canalización y agregando los metadatos del punto de conexión o limite el conjunto de orígenes permitidos [configurando SignalR el uso compartido](xref:signalr/security#cross-origin-resource-sharing)de recursos entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-423">To disable cross-origin access, either disable CORS in the endpoint by adding the CORS middleware to the pipeline and adding the `DisableCorsAttribute` to the Blazor endpoint metadata or limit the set of allowed origins by [configuring SignalR for cross-origin resource sharing](xref:signalr/security#cross-origin-resource-sharing).</span></span>
* <span data-ttu-id="2ccf5-424">Si CORS está habilitado, es posible que se necesiten pasos adicionales para proteger la aplicación en función de la configuración de CORS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-424">If CORS is enabled, extra steps might be required to protect the app depending on the CORS configuration.</span></span> <span data-ttu-id="2ccf5-425">Si CORS está habilitado globalmente, CORS Blazor se puede `DisableCorsAttribute` deshabilitar para el concentrador `hub.MapBlazorHub()`de servidor agregando los metadatos a los metadatos del punto de conexión después de llamar a .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-425">If CORS is globally enabled, CORS can be disabled for the Blazor Server hub by adding the `DisableCorsAttribute` metadata to the endpoint metadata after calling `hub.MapBlazorHub()`.</span></span>

<span data-ttu-id="2ccf5-426">Para obtener más información, vea <xref:security/anti-request-forgery>.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-426">For more information, see <xref:security/anti-request-forgery>.</span></span>

### <a name="click-jacking"></a><span data-ttu-id="2ccf5-427">Click-jacking</span><span class="sxs-lookup"><span data-stu-id="2ccf5-427">Click-jacking</span></span>

<span data-ttu-id="2ccf5-428">El click-jacking implica representar `<iframe>` un sitio como un sitio dentro de un origen diferente con el fin de engañar al usuario para que realice acciones en el sitio bajo ataque.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-428">Click-jacking involves rendering a site as an `<iframe>` inside a site from a different origin in order to trick the user into performing actions on the site under attack.</span></span>

<span data-ttu-id="2ccf5-429">Para proteger una aplicación de `<iframe>`la representación dentro de `X-Frame-Options` una , use la directiva de seguridad de contenido [(CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) y el encabezado.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-429">To protect an app from rendering inside of an `<iframe>`, use [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) and the `X-Frame-Options` header.</span></span> <span data-ttu-id="2ccf5-430">Para obtener más información, consulte Documentos web de [MDN: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).</span><span class="sxs-lookup"><span data-stu-id="2ccf5-430">For more information, see [MDN web docs: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).</span></span>

### <a name="open-redirects"></a><span data-ttu-id="2ccf5-431">Redirecciones abiertas</span><span class="sxs-lookup"><span data-stu-id="2ccf5-431">Open redirects</span></span>

<span data-ttu-id="2ccf5-432">Cuando Blazor se inicia una sesión de aplicación de servidor, el servidor realiza la validación básica de las direcciones URL enviadas como parte del inicio de la sesión.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-432">When a Blazor Server app session starts, the server performs basic validation of the URLs sent as part of starting the session.</span></span> <span data-ttu-id="2ccf5-433">El marco de trabajo comprueba que la dirección URL base es un elemento primario de la dirección URL actual antes de establecer el circuito.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-433">The framework checks that the base URL is a parent of the current URL before establishing the circuit.</span></span> <span data-ttu-id="2ccf5-434">El marco de trabajo no realiza comprobaciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-434">No additional checks are performed by the framework.</span></span>

<span data-ttu-id="2ccf5-435">Cuando un usuario selecciona un vínculo en el cliente, la dirección URL del vínculo se envía al servidor, que determina qué acción realizar.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-435">When a user selects a link on the client, the URL for the link is sent to the server, which determines what action to take.</span></span> <span data-ttu-id="2ccf5-436">Por ejemplo, la aplicación puede realizar una navegación del lado cliente o indicar al explorador para ir a la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-436">For example, the app may perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="2ccf5-437">Los componentes también pueden desencadenar solicitudes `NavigationManager`de navegación mediante programación mediante el uso de .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-437">Components can also trigger navigation requests programatically through the use of `NavigationManager`.</span></span> <span data-ttu-id="2ccf5-438">En tales escenarios, la aplicación puede realizar una navegación del lado cliente o indicar al explorador para ir a la nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-438">In such scenarios, the app might perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="2ccf5-439">Los componentes deben:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-439">Components must:</span></span>

* <span data-ttu-id="2ccf5-440">Evite usar la entrada del usuario como parte de los argumentos de llamada de navegación.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-440">Avoid using user input as part of the navigation call arguments.</span></span>
* <span data-ttu-id="2ccf5-441">Validar argumentos para asegurarse de que la aplicación permite el destino.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-441">Validate arguments to ensure that the target is allowed by the app.</span></span>

<span data-ttu-id="2ccf5-442">De lo contrario, un usuario malintencionado puede forzar al navegador a ir a un sitio controlado por el atacante.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-442">Otherwise, a malicious user can force the browser to go to an attacker-controlled site.</span></span> <span data-ttu-id="2ccf5-443">En este escenario, el atacante engaña a la aplicación para `NavigationManager.Navigate` que use alguna entrada de usuario como parte de la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-443">In this scenario, the attacker tricks the app into using some user input as part of the invocation of the `NavigationManager.Navigate` method.</span></span>

<span data-ttu-id="2ccf5-444">Este consejo también se aplica al representar vínculos como parte de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-444">This advice also applies when rendering links as part of the app:</span></span>

* <span data-ttu-id="2ccf5-445">Si es posible, utilice enlaces relativos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-445">If possible, use relative links.</span></span>
* <span data-ttu-id="2ccf5-446">Valide que los destinos de vínculo absolutos son válidos antes de incluirlos en una página.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-446">Validate that absolute link destinations are valid before including them in a page.</span></span>

<span data-ttu-id="2ccf5-447">Para obtener más información, vea <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-447">For more information, see <xref:security/preventing-open-redirects>.</span></span>

## <a name="authentication-and-authorization"></a><span data-ttu-id="2ccf5-448">Autenticación y autorización</span><span class="sxs-lookup"><span data-stu-id="2ccf5-448">Authentication and authorization</span></span>

<span data-ttu-id="2ccf5-449">Para obtener instrucciones sobre autenticación <xref:security/blazor/index>y autorización, consulte .</span><span class="sxs-lookup"><span data-stu-id="2ccf5-449">For guidance on authentication and authorization, see <xref:security/blazor/index>.</span></span>

## <a name="security-checklist"></a><span data-ttu-id="2ccf5-450">Lista de comprobación de seguridad</span><span class="sxs-lookup"><span data-stu-id="2ccf5-450">Security checklist</span></span>

<span data-ttu-id="2ccf5-451">La siguiente lista de consideraciones de seguridad no es completa:</span><span class="sxs-lookup"><span data-stu-id="2ccf5-451">The following list of security considerations isn't comprehensive:</span></span>

* <span data-ttu-id="2ccf5-452">Validar argumentos de eventos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-452">Validate arguments from events.</span></span>
* <span data-ttu-id="2ccf5-453">Valide las entradas y los resultados de las llamadas de interoperabilidad de JS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-453">Validate inputs and results from JS interop calls.</span></span>
* <span data-ttu-id="2ccf5-454">Evite usar (o validar de antemano) la entrada de usuario para las llamadas de interoperabilidad de .NET a JS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-454">Avoid using (or validate beforehand) user input for .NET to JS interop calls.</span></span>
* <span data-ttu-id="2ccf5-455">Evite que el cliente amese una cantidad de memoria sin enlazar.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-455">Prevent the client from allocating an unbound amount of memory.</span></span>
  * <span data-ttu-id="2ccf5-456">Datos dentro del componente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-456">Data within the component.</span></span>
  * <span data-ttu-id="2ccf5-457">`DotNetObject`referencias devueltas al cliente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-457">`DotNetObject` references returned to the client.</span></span>
* <span data-ttu-id="2ccf5-458">Protéyate de múltiples despachos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-458">Guard against multiple dispatches.</span></span>
* <span data-ttu-id="2ccf5-459">Cancelar operaciones de ejecución prolongada cuando se elimina el componente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-459">Cancel long-running operations when the component is disposed.</span></span>
* <span data-ttu-id="2ccf5-460">Evite eventos que produzcan grandes cantidades de datos.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-460">Avoid events that produce large amounts of data.</span></span>
* <span data-ttu-id="2ccf5-461">Evite usar la entrada del `NavigationManager.Navigate` usuario como parte de las llamadas y valide la entrada del usuario para las direcciones URL con un conjunto de orígenes permitidos primero si es inevitable.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-461">Avoid using user input as part of calls to `NavigationManager.Navigate` and validate user input for URLs against a set of allowed origins first if unavoidable.</span></span>
* <span data-ttu-id="2ccf5-462">No tome decisiones de autorización basadas en el estado de la interfaz de usuario, sino solo desde el estado del componente.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-462">Don't make authorization decisions based on the state of the UI but only from component state.</span></span>
* <span data-ttu-id="2ccf5-463">Considere la posibilidad de usar [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) para protegerse contra ataques XSS.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-463">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to protect against XSS attacks.</span></span>
* <span data-ttu-id="2ccf5-464">Considere la posibilidad de usar CSP y [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) para proteger contra el robo de clics.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-464">Consider using CSP and [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) to protect against click-jacking.</span></span>
* <span data-ttu-id="2ccf5-465">Asegúrese de que la configuración de CORS sea Blazor adecuada al habilitar CORS o deshabilitar explícitamente CORS para aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-465">Ensure CORS settings are appropriate when enabling CORS or explicitly disable CORS for Blazor apps.</span></span>
* <span data-ttu-id="2ccf5-466">Pruebe para asegurarse de que Blazor los límites del lado del servidor para la aplicación proporcionan una experiencia de usuario aceptable sin niveles de riesgo inaceptables.</span><span class="sxs-lookup"><span data-stu-id="2ccf5-466">Test to ensure that the server-side limits for the Blazor app provide an acceptable user experience without unacceptable levels of risk.</span></span>
