---
title: Host web de ASP.NET Core
author: guardrex
description: Obtenga información sobre el host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7440ab26534840b190a346614f645860fc2b7d78
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089904"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="57d62-103">Host web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57d62-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="57d62-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="57d62-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="57d62-105">Las aplicaciones de ASP.NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="57d62-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="57d62-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="57d62-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="57d62-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="57d62-108">En este tema se aborda el host web de ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), muy útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="57d62-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="57d62-109">Para conocer la cobertura del host genérico de .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vea <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="57d62-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="57d62-110">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="57d62-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="57d62-111">Cree un host con una instancia de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="57d62-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="57d62-112">Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="57d62-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="57d62-113">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="57d62-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="57d62-114">Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="57d62-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="57d62-115">`CreateDefaultBuilder` realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="57d62-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="57d62-116">Configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y configura el servidor por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="57d62-117">Para consultar las opciones predeterminadas de Kestrel, vea <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="57d62-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="57d62-118">Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="57d62-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="57d62-119">Carga la [configuración de host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="57d62-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="57d62-120">Variables de entorno con el prefijo `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="57d62-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="57d62-121">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="57d62-121">Command-line arguments.</span></span>
* <span data-ttu-id="57d62-122">Carga la configuración de aplicación de:</span><span class="sxs-lookup"><span data-stu-id="57d62-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="57d62-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="57d62-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="57d62-124">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="57d62-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="57d62-125">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="57d62-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="57d62-126">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="57d62-126">Environment variables.</span></span>
  * <span data-ttu-id="57d62-127">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="57d62-127">Command-line arguments.</span></span>
* <span data-ttu-id="57d62-128">Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="57d62-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="57d62-129">El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="57d62-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="57d62-130">Cuando se ejecuta detrás de IIS, permite la [integración con IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="57d62-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="57d62-131">Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="57d62-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="57d62-132">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="57d62-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="57d62-133">También configura la aplicación para que [capture errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="57d62-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="57d62-134">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="57d62-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="57d62-135">Establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="57d62-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="57d62-136">Para más información, vea [Validación del ámbito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="57d62-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="57d62-137">La configuración definida en `CreateDefaultBuilder` se puede reemplazar y aumentar mediante [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) y otros métodos y métodos de extensión de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="57d62-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="57d62-138">A continuación, se presentan algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="57d62-138">A few examples follow:</span></span>

* <span data-ttu-id="57d62-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) se usa para especificar `IConfiguration` adicionales para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="57d62-140">La siguiente llamada `ConfigureAppConfiguration` agrega un delegado para incluir la configuración de la aplicación en el archivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="57d62-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="57d62-141">Es posible llamar a `ConfigureAppConfiguration` varias veces.</span><span class="sxs-lookup"><span data-stu-id="57d62-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="57d62-142">Tenga en cuenta que esta configuración no se aplica al host (por ejemplo, direcciones URL de servidor o entorno).</span><span class="sxs-lookup"><span data-stu-id="57d62-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="57d62-143">Vea la sección [Valores de configuración de host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="57d62-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="57d62-144">La siguiente llamada `ConfigureLogging` agrega un delegado para configurar el nivel de registro mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) en [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="57d62-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="57d62-145">Esta configuración reemplaza la configuración de *appsettings.Development.JSON* (`LogLevel.Debug`) y *appsettings.Production.JSON* (`LogLevel.Error`) configurada mediante `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="57d62-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="57d62-146">Es posible llamar a `ConfigureLogging` varias veces.</span><span class="sxs-lookup"><span data-stu-id="57d62-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="57d62-147">La siguiente llamada a `ConfigureKestrel` reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57d62-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="57d62-148">La siguiente llamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57d62-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="57d62-149">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="57d62-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="57d62-150">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="57d62-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="57d62-151">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="57d62-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="57d62-152">Para obtener más información sobre la configuración de la aplicación, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="57d62-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="57d62-153">Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="57d62-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="57d62-154">Para más información, vea la pestaña ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="57d62-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="57d62-155">Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="57d62-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="57d62-156">La creación de un host se suele realizar en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="57d62-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="57d62-157">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="57d62-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="57d62-158">`WebHostBuilder` requiere un [servidor que implemente IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="57d62-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="57d62-159">Los servidores integrados son [Kestrel](xref:fundamentals/servers/kestrel) y [HTTP.sys](xref:fundamentals/servers/httpsys) (antes del lanzamiento de ASP.NET Core 2.0, HTTP.sys se llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="57d62-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="57d62-160">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="57d62-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="57d62-161">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="57d62-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="57d62-162">La raíz de contenido predeterminada se obtiene para `UseContentRoot` mediante [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="57d62-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="57d62-163">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="57d62-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="57d62-164">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="57d62-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="57d62-165">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="57d62-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="57d62-166">A diferencia de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` no configura un *servidor*.</span><span class="sxs-lookup"><span data-stu-id="57d62-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="57d62-167">`UseIISIntegration` configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="57d62-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="57d62-168">Para utilizar IIS con ASP.NET Core, deben especificarse `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="57d62-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="57d62-169">`UseIISIntegration` solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="57d62-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="57d62-170">Para obtener más información, consulte <xref:fundamentals/servers/aspnet-core-module> y <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="57d62-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="57d62-171">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de la canalización de solicitudes de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="57d62-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

::: moniker-end

<span data-ttu-id="57d62-172">Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="57d62-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="57d62-173">Si se especifica una clase `Startup`, debe definir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="57d62-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="57d62-174">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="57d62-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="57d62-175">Varias llamadas a `ConfigureServices` se anexan entre sí.</span><span class="sxs-lookup"><span data-stu-id="57d62-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="57d62-176">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="57d62-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="57d62-177">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="57d62-177">Host configuration values</span></span>

<span data-ttu-id="57d62-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="57d62-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="57d62-179">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="57d62-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="57d62-180">Por ejemplo: `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="57d62-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="57d62-181">Extensiones como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) y [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vea la sección [Invalidación de la configuración](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="57d62-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="57d62-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="57d62-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="57d62-183">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="57d62-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="57d62-184">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="57d62-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="57d62-185">Para obtener más información, consulte [Invalidación de la configuración](#override-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="57d62-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="57d62-186">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="57d62-186">Application Key (Name)</span></span>

<span data-ttu-id="57d62-187">La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece automáticamente cuando se llama a [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la construcción del host.</span><span class="sxs-lookup"><span data-stu-id="57d62-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="57d62-188">El valor se establece en el nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="57d62-189">Para establecer el valor explícitamente, use [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="57d62-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="57d62-190">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="57d62-190">**Key**: applicationName</span></span>  
<span data-ttu-id="57d62-191">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-191">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-192">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="57d62-193">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57d62-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57d62-194">**Variable de entorno**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="57d62-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="57d62-195">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="57d62-195">Capture Startup Errors</span></span>

<span data-ttu-id="57d62-196">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="57d62-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="57d62-197">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="57d62-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="57d62-198">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="57d62-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57d62-199">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="57d62-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="57d62-200">**Establecer mediante**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="57d62-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="57d62-201">**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="57d62-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="57d62-202">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="57d62-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="57d62-203">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="57d62-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="57d62-204">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="57d62-204">Content Root</span></span>

<span data-ttu-id="57d62-205">Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="57d62-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="57d62-206">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="57d62-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="57d62-207">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-207">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-208">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="57d62-209">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="57d62-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="57d62-210">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="57d62-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="57d62-211">La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="57d62-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="57d62-212">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="57d62-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="57d62-213">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="57d62-213">Detailed Errors</span></span>

<span data-ttu-id="57d62-214">Determina si se deben capturar los errores detallados.</span><span class="sxs-lookup"><span data-stu-id="57d62-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="57d62-215">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="57d62-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="57d62-216">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="57d62-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57d62-217">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="57d62-217">**Default**: false</span></span>  
<span data-ttu-id="57d62-218">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57d62-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57d62-219">**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="57d62-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="57d62-220">Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="57d62-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="57d62-221">Entorno</span><span class="sxs-lookup"><span data-stu-id="57d62-221">Environment</span></span>

<span data-ttu-id="57d62-222">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-222">Sets the app's environment.</span></span>

<span data-ttu-id="57d62-223">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="57d62-223">**Key**: environment</span></span>  
<span data-ttu-id="57d62-224">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-224">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-225">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="57d62-225">**Default**: Production</span></span>  
<span data-ttu-id="57d62-226">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="57d62-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="57d62-227">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="57d62-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="57d62-228">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="57d62-228">The environment can be set to any value.</span></span> <span data-ttu-id="57d62-229">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="57d62-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="57d62-230">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="57d62-230">Values aren't case sensitive.</span></span> <span data-ttu-id="57d62-231">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="57d62-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="57d62-232">Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="57d62-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="57d62-233">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="57d62-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="57d62-234">Ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="57d62-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="57d62-235">Establece los ensamblados de inicio de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="57d62-236">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="57d62-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="57d62-237">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-237">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-238">**Valor predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="57d62-238">**Default**: Empty string</span></span>  
<span data-ttu-id="57d62-239">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57d62-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57d62-240">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="57d62-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="57d62-241">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="57d62-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="57d62-242">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="57d62-243">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="57d62-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="57d62-244">Puerto HTTPS</span><span class="sxs-lookup"><span data-stu-id="57d62-244">HTTPS Port</span></span>

<span data-ttu-id="57d62-245">Establezca puerto de redirección HTTPS.</span><span class="sxs-lookup"><span data-stu-id="57d62-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="57d62-246">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="57d62-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="57d62-247">**Clave**: https_port **Tipo**: *string*
**Valor predeterminada**: no se establece un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="57d62-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="57d62-248">**Establecer mediante**: `UseSetting`
**Variable de entorno**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="57d62-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="57d62-249">Ensamblados de exclusión de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="57d62-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="57d62-250">DESCRIPTION</span><span class="sxs-lookup"><span data-stu-id="57d62-250">DESCRIPTION</span></span>

<span data-ttu-id="57d62-251">**Clave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="57d62-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="57d62-252">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-252">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-253">**Valor predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="57d62-253">**Default**: Empty string</span></span>  
<span data-ttu-id="57d62-254">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57d62-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57d62-255">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="57d62-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="57d62-256">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="57d62-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="57d62-257">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="57d62-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="57d62-258">Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="57d62-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="57d62-259">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="57d62-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="57d62-260">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="57d62-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57d62-261">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="57d62-261">**Default**: true</span></span>  
<span data-ttu-id="57d62-262">**Establecer mediante**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="57d62-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="57d62-263">**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="57d62-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="57d62-264">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="57d62-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="57d62-265">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="57d62-266">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="57d62-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="57d62-267">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="57d62-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="57d62-268">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="57d62-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57d62-269">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="57d62-269">**Default**: false</span></span>  
<span data-ttu-id="57d62-270">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57d62-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57d62-271">**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="57d62-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="57d62-272">Direcciones URL de servidor</span><span class="sxs-lookup"><span data-stu-id="57d62-272">Server URLs</span></span>

<span data-ttu-id="57d62-273">Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="57d62-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="57d62-274">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="57d62-274">**Key**: urls</span></span>  
<span data-ttu-id="57d62-275">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-275">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-276">**Predeterminado**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="57d62-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="57d62-277">**Establecer mediante**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="57d62-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="57d62-278">**Variable de entorno**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="57d62-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="57d62-279">Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="57d62-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="57d62-280">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="57d62-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="57d62-281">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="57d62-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="57d62-282">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="57d62-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="57d62-283">Los formatos admitidos varían en función del servidor.</span><span class="sxs-lookup"><span data-stu-id="57d62-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="57d62-284">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="57d62-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="57d62-285">Para obtener más información, vea <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="57d62-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="57d62-286">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="57d62-286">Shutdown Timeout</span></span>

<span data-ttu-id="57d62-287">Especifica la cantidad de tiempo que se espera hasta el cierre del host de web.</span><span class="sxs-lookup"><span data-stu-id="57d62-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="57d62-288">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="57d62-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="57d62-289">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="57d62-289">**Type**: *int*</span></span>  
<span data-ttu-id="57d62-290">**Valor predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="57d62-290">**Default**: 5</span></span>  
<span data-ttu-id="57d62-291">**Establecer mediante**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="57d62-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="57d62-292">**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="57d62-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="57d62-293">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) toma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="57d62-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="57d62-294">Durante el período de tiempo de espera, el hospedaje:</span><span class="sxs-lookup"><span data-stu-id="57d62-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="57d62-295">Activa [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="57d62-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="57d62-296">Trata de detener los servicios hospedados y registra cualquier error que se produzca en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="57d62-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="57d62-297">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="57d62-298">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="57d62-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="57d62-299">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="57d62-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="57d62-300">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="57d62-300">Startup Assembly</span></span>

<span data-ttu-id="57d62-301">Determina el ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="57d62-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="57d62-302">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="57d62-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="57d62-303">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-303">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-304">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="57d62-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="57d62-305">**Establecer mediante**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="57d62-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="57d62-306">**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="57d62-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="57d62-307">Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="57d62-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="57d62-308">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="57d62-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="57d62-309">Raíz web</span><span class="sxs-lookup"><span data-stu-id="57d62-309">Web Root</span></span>

<span data-ttu-id="57d62-310">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="57d62-311">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="57d62-311">**Key**: webroot</span></span>  
<span data-ttu-id="57d62-312">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="57d62-312">**Type**: *string*</span></span>  
<span data-ttu-id="57d62-313">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="57d62-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="57d62-314">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="57d62-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="57d62-315">**Establecer mediante**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="57d62-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="57d62-316">**Variable de entorno**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="57d62-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="57d62-317">Invalidación de la configuración</span><span class="sxs-lookup"><span data-stu-id="57d62-317">Override configuration</span></span>

<span data-ttu-id="57d62-318">Use [Configuración](xref:fundamentals/configuration/index) para configurar el host web.</span><span class="sxs-lookup"><span data-stu-id="57d62-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="57d62-319">En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="57d62-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="57d62-320">Cualquier configuración que se cargue del archivo *hostsettings.json* se puede reemplazar por argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="57d62-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="57d62-321">La configuración generada (en `config`) se usa para configurar el host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="57d62-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="57d62-322">La configuración de `IWebHostBuilder` se agrega a la de la aplicación, pero lo contrario no es aplicable: `ConfigureAppConfiguration` no afecta a la configuración de `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="57d62-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="57d62-323">Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="57d62-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

<span data-ttu-id="57d62-324">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="57d62-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="57d62-325">Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="57d62-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

<span data-ttu-id="57d62-326">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="57d62-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="57d62-327">El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="57d62-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="57d62-328">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="57d62-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="57d62-329">El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="57d62-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="57d62-330">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="57d62-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="57d62-331">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="57d62-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="57d62-332">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="57d62-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="57d62-333">`UseConfiguration` solo copia las claves del elemento `IConfiguration` proporcionado a la configuración del generador de host.</span><span class="sxs-lookup"><span data-stu-id="57d62-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="57d62-334">Por consiguiente, el hecho de configurar `reloadOnChange: true` para los archivos de configuración XML, JSON e INI no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="57d62-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="57d62-335">Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="57d62-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="57d62-336">El argumento de línea de comandos reemplaza el valor `urls` del archivo *hostsettings.json*, y el servidor efectúa la escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="57d62-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="57d62-337">Administración del host</span><span class="sxs-lookup"><span data-stu-id="57d62-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="57d62-338">**Run**</span><span class="sxs-lookup"><span data-stu-id="57d62-338">**Run**</span></span>

<span data-ttu-id="57d62-339">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="57d62-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="57d62-340">**Start**</span><span class="sxs-lookup"><span data-stu-id="57d62-340">**Start**</span></span>

<span data-ttu-id="57d62-341">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="57d62-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="57d62-342">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="57d62-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="57d62-343">La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático.</span><span class="sxs-lookup"><span data-stu-id="57d62-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="57d62-344">Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="57d62-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="57d62-345">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="57d62-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="57d62-346">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="57d62-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-347">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="57d62-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="57d62-348">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="57d62-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57d62-349">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="57d62-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57d62-350">**Start(url de cadena, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="57d62-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="57d62-351">Start con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="57d62-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-352">Produce el mismo resultado que **Start(RequestDelegate app)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57d62-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="57d62-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="57d62-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="57d62-354">Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="57d62-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-355">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="57d62-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="57d62-356">Solicitud</span><span class="sxs-lookup"><span data-stu-id="57d62-356">Request</span></span>                                    | <span data-ttu-id="57d62-357">Respuesta</span><span class="sxs-lookup"><span data-stu-id="57d62-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="57d62-358">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="57d62-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="57d62-359">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="57d62-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="57d62-360">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="57d62-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="57d62-361">Produce una excepción con la cadena "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="57d62-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="57d62-362">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="57d62-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="57d62-363">Hello World!</span><span class="sxs-lookup"><span data-stu-id="57d62-363">Hello World!</span></span>                             |

<span data-ttu-id="57d62-364">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="57d62-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57d62-365">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="57d62-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57d62-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="57d62-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="57d62-367">Use una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57d62-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-368">Produce el mismo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57d62-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="57d62-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="57d62-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="57d62-370">Proporciona un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57d62-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-371">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="57d62-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="57d62-372">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="57d62-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57d62-373">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="57d62-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57d62-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="57d62-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="57d62-375">Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57d62-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57d62-376">Produce el mismo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57d62-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="57d62-377">**Run**</span><span class="sxs-lookup"><span data-stu-id="57d62-377">**Run**</span></span>

<span data-ttu-id="57d62-378">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="57d62-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="57d62-379">**Start**</span><span class="sxs-lookup"><span data-stu-id="57d62-379">**Start**</span></span>

<span data-ttu-id="57d62-380">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="57d62-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="57d62-381">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="57d62-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="57d62-382">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="57d62-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="57d62-383">La [interfaz IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="57d62-384">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="57d62-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="57d62-385">Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#environment-based-startup-class-and-methods) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="57d62-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="57d62-386">Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="57d62-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="57d62-387">Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="57d62-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="57d62-388">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="57d62-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="57d62-389">El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="57d62-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="57d62-390">`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/index#write-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="57d62-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="57d62-391">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="57d62-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="57d62-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="57d62-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="57d62-393">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="57d62-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="57d62-394">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="57d62-394">Cancellation Token</span></span>    | <span data-ttu-id="57d62-395">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="57d62-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="57d62-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="57d62-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="57d62-397">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="57d62-397">The host has fully started.</span></span> |
| [<span data-ttu-id="57d62-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="57d62-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="57d62-399">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="57d62-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="57d62-400">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="57d62-400">All requests should be processed.</span></span> <span data-ttu-id="57d62-401">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="57d62-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="57d62-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="57d62-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="57d62-403">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="57d62-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="57d62-404">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="57d62-404">Requests may still be processing.</span></span> <span data-ttu-id="57d62-405">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="57d62-405">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="57d62-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57d62-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="57d62-407">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="57d62-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a><span data-ttu-id="57d62-408">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="57d62-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="57d62-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="57d62-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="57d62-410">Cuando `ValidateScopes` está establecido en `true`, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="57d62-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="57d62-411">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="57d62-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="57d62-412">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="57d62-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="57d62-413">El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="57d62-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="57d62-414">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="57d62-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="57d62-415">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="57d62-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="57d62-416">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="57d62-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="57d62-417">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="57d62-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="57d62-418">Para validar los ámbitos siempre, incluso en el entorno de producción, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) en el generador de hosts:</span><span class="sxs-lookup"><span data-stu-id="57d62-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="57d62-419">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="57d62-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="57d62-420">**Lo siguiente solo es pertinente en las aplicaciones de ASP.NET Core 2.0 cuando la aplicación no llama a `UseStartup` o `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="57d62-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="57d62-421">Se puede crear un host mediante la inserción directa de `IStartup` en el contenedor de inserción de dependencias en lugar de llamar a `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="57d62-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="57d62-422">Si el host se crea de esta forma, puede producirse el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="57d62-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="57d62-423">Esto ocurre porque es necesario el nombre de la aplicación (el nombre del ensamblado actual) para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="57d62-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="57d62-424">Si la aplicación inserta manualmente `IStartup` en el contenedor de inserción de dependencias, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="57d62-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="57d62-425">O bien, agregue un `Configure` ficticio a `WebHostBuilder`, que establece el nombre de la aplicación automáticamente:</span><span class="sxs-lookup"><span data-stu-id="57d62-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="57d62-426">Para obtener más información, vea [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario) y el [ejemplo StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="57d62-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="57d62-427">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="57d62-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
