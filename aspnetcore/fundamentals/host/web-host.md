---
title: Host web de ASP.NET Core
author: guardrex
description: Obtenga información sobre el host web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: d387098662cc832cc0e49b6a1636f0ebcc7308de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081690"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="f92a6-103">Host web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f92a6-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="f92a6-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f92a6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f92a6-105">Las aplicaciones de ASP.NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="f92a6-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f92a6-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f92a6-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="f92a6-108">El host también puede configurar el registro, la inserción de dependencias y la configuración.</span><span class="sxs-lookup"><span data-stu-id="f92a6-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f92a6-109">En el artículo se explica el host web, que solo permanece disponible para compatibilidad con versiones anteriores:</span><span class="sxs-lookup"><span data-stu-id="f92a6-109">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="f92a6-110">Se recomienda el [host genérico](xref:fundamentals/host/generic-host) para todos los tipos de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="f92a6-110">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="f92a6-111">En este artículo se describe el host web, que sirve para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="f92a6-111">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="f92a6-112">Para otros tipos de aplicaciones, use el [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="f92a6-112">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="f92a6-113">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="f92a6-113">Set up a host</span></span>

<span data-ttu-id="f92a6-114">Cree un host con una instancia de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f92a6-114">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f92a6-115">Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-115">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="f92a6-116">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-116">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="f92a6-117">Una aplicación típica llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="f92a6-117">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="f92a6-118">El código que llama a `CreateDefaultBuilder` está en un método denominado `CreateWebHostBuilder`, que lo diferencia del código de `Main` que llama a `Run` en el objeto generador.</span><span class="sxs-lookup"><span data-stu-id="f92a6-118">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="f92a6-119">Dicha diferencia es necesaria si usa [herramientas de Entity Framework Core](/ef/core/miscellaneous/cli/).</span><span class="sxs-lookup"><span data-stu-id="f92a6-119">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="f92a6-120">Las herramientas esperan encontrar un método `CreateWebHostBuilder` al que pueden llamar en tiempo de diseño para configurar el host sin ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-120">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="f92a6-121">Una alternativa consiste en implementar `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-121">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="f92a6-122">Para obtener más información, consulte [Creación de DbContext en tiempo de diseño](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="f92a6-122">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="f92a6-123">`CreateDefaultBuilder` realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="f92a6-123">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="f92a6-124">Configura el servidor [Kestrel](xref:fundamentals/servers/kestrel) como servidor web por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-124">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="f92a6-125">Para conocer las opciones predeterminadas del servidor Kestrel, consulte <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-125">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="f92a6-126">Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="f92a6-126">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="f92a6-127">Carga la [configuración de host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="f92a6-127">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="f92a6-128">Variables de entorno con el prefijo `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="f92a6-128">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="f92a6-129">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f92a6-129">Command-line arguments.</span></span>
* <span data-ttu-id="f92a6-130">Carga la configuración de la aplicación en el siguiente orden:</span><span class="sxs-lookup"><span data-stu-id="f92a6-130">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="f92a6-131">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-131">*appsettings.json*.</span></span>
  * <span data-ttu-id="f92a6-132">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-132">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="f92a6-133">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="f92a6-133">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f92a6-134">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f92a6-134">Environment variables.</span></span>
  * <span data-ttu-id="f92a6-135">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f92a6-135">Command-line arguments.</span></span>
* <span data-ttu-id="f92a6-136">Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="f92a6-136">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="f92a6-137">El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-137">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="f92a6-138">Cuando se ejecuta detrás de IIS con el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` habilita la [integración de IIS](xref:host-and-deploy/iis/index), que configura la dirección base y el puerto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-138">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="f92a6-139">La integración de IIS también configura la aplicación para que [capture errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="f92a6-139">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="f92a6-140">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-140">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="f92a6-141">Establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f92a6-141">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="f92a6-142">Para más información, vea [Validación del ámbito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="f92a6-142">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="f92a6-143">La configuración definida en `CreateDefaultBuilder` se puede reemplazar y aumentar mediante [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) y otros métodos y métodos de extensión de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f92a6-143">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f92a6-144">A continuación, se presentan algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="f92a6-144">A few examples follow:</span></span>

* <span data-ttu-id="f92a6-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) se usa para especificar `IConfiguration` adicionales para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="f92a6-146">La siguiente llamada `ConfigureAppConfiguration` agrega un delegado para incluir la configuración de la aplicación en el archivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-146">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="f92a6-147">Es posible llamar a `ConfigureAppConfiguration` varias veces.</span><span class="sxs-lookup"><span data-stu-id="f92a6-147">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="f92a6-148">Tenga en cuenta que esta configuración no se aplica al host (por ejemplo, direcciones URL de servidor o entorno).</span><span class="sxs-lookup"><span data-stu-id="f92a6-148">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="f92a6-149">Vea la sección [Valores de configuración de host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="f92a6-149">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="f92a6-150">La siguiente llamada `ConfigureLogging` agrega un delegado para configurar el nivel de registro mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) en [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="f92a6-150">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="f92a6-151">Esta configuración reemplaza la configuración de *appsettings.Development.JSON* (`LogLevel.Debug`) y *appsettings.Production.JSON* (`LogLevel.Error`) configurada mediante `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-151">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f92a6-152">Es posible llamar a `ConfigureLogging` varias veces.</span><span class="sxs-lookup"><span data-stu-id="f92a6-152">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f92a6-153">La siguiente llamada a `ConfigureKestrel` reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-153">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f92a6-154">La siguiente llamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-154">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="f92a6-155">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="f92a6-155">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="f92a6-156">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="f92a6-156">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="f92a6-157">Este es el valor predeterminado usado en [Visual Studio](https://visualstudio.microsoft.com) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="f92a6-157">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="f92a6-158">Para obtener más información sobre la configuración de la aplicación, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-158">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="f92a6-159">Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="f92a6-159">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="f92a6-160">Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices).</span><span class="sxs-lookup"><span data-stu-id="f92a6-160">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="f92a6-161">Si se especifica una clase `Startup`, debe definir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-161">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="f92a6-162">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-162">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="f92a6-163">Varias llamadas a `ConfigureServices` se anexan entre sí.</span><span class="sxs-lookup"><span data-stu-id="f92a6-163">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="f92a6-164">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="f92a6-164">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f92a6-165">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="f92a6-165">Host configuration values</span></span>

<span data-ttu-id="f92a6-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="f92a6-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="f92a6-167">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-167">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="f92a6-168">Por ejemplo: `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-168">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="f92a6-169">Extensiones como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) y [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vea la sección [Invalidación de la configuración](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="f92a6-169">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="f92a6-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="f92a6-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="f92a6-171">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="f92a6-171">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="f92a6-172">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="f92a6-172">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="f92a6-173">Para obtener más información, consulte [Invalidación de la configuración](#override-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="f92a6-173">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="f92a6-174">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="f92a6-174">Application Key (Name)</span></span>

<span data-ttu-id="f92a6-175">La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece automáticamente cuando se llama a [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la construcción del host.</span><span class="sxs-lookup"><span data-stu-id="f92a6-175">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="f92a6-176">El valor se establece en el nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-176">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="f92a6-177">Para establecer el valor explícitamente, use [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="f92a6-177">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="f92a6-178">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="f92a6-178">**Key**: applicationName</span></span>  
<span data-ttu-id="f92a6-179">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-179">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-180">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-180">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="f92a6-181">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f92a6-181">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f92a6-182">**Variable de entorno**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="f92a6-182">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="f92a6-183">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="f92a6-183">Capture Startup Errors</span></span>

<span data-ttu-id="f92a6-184">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="f92a6-184">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="f92a6-185">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="f92a6-185">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="f92a6-186">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="f92a6-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f92a6-187">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-187">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="f92a6-188">**Establecer mediante**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="f92a6-188">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="f92a6-189">**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="f92a6-189">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="f92a6-190">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="f92a6-190">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="f92a6-191">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="f92a6-191">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="f92a6-192">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="f92a6-192">Content Root</span></span>

<span data-ttu-id="f92a6-193">Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="f92a6-193">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="f92a6-194">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="f92a6-194">**Key**: contentRoot</span></span>  
<span data-ttu-id="f92a6-195">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-195">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-196">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-196">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="f92a6-197">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="f92a6-197">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="f92a6-198">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="f92a6-198">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="f92a6-199">La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="f92a6-199">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="f92a6-200">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="f92a6-200">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="f92a6-201">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="f92a6-201">Detailed Errors</span></span>

<span data-ttu-id="f92a6-202">Determina si se deben capturar los errores detallados.</span><span class="sxs-lookup"><span data-stu-id="f92a6-202">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="f92a6-203">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="f92a6-203">**Key**: detailedErrors</span></span>  
<span data-ttu-id="f92a6-204">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="f92a6-204">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f92a6-205">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="f92a6-205">**Default**: false</span></span>  
<span data-ttu-id="f92a6-206">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f92a6-206">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f92a6-207">**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="f92a6-207">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="f92a6-208">Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="f92a6-208">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="f92a6-209">Entorno</span><span class="sxs-lookup"><span data-stu-id="f92a6-209">Environment</span></span>

<span data-ttu-id="f92a6-210">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-210">Sets the app's environment.</span></span>

<span data-ttu-id="f92a6-211">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="f92a6-211">**Key**: environment</span></span>  
<span data-ttu-id="f92a6-212">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-212">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-213">**Valor predeterminado**: Producción</span><span class="sxs-lookup"><span data-stu-id="f92a6-213">**Default**: Production</span></span>  
<span data-ttu-id="f92a6-214">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="f92a6-214">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="f92a6-215">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="f92a6-215">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="f92a6-216">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="f92a6-216">The environment can be set to any value.</span></span> <span data-ttu-id="f92a6-217">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="f92a6-218">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f92a6-218">Values aren't case sensitive.</span></span> <span data-ttu-id="f92a6-219">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-219">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="f92a6-220">Cuando se usa [Visual Studio](https://visualstudio.microsoft.com), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-220">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="f92a6-221">Para más información, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-221">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="f92a6-222">Ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="f92a6-222">Hosting Startup Assemblies</span></span>

<span data-ttu-id="f92a6-223">Establece los ensamblados de inicio de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-223">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="f92a6-224">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="f92a6-224">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="f92a6-225">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-225">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-226">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="f92a6-226">**Default**: Empty string</span></span>  
<span data-ttu-id="f92a6-227">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f92a6-227">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f92a6-228">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="f92a6-228">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="f92a6-229">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="f92a6-229">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="f92a6-230">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-230">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="f92a6-231">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="f92a6-231">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="f92a6-232">Puerto HTTPS</span><span class="sxs-lookup"><span data-stu-id="f92a6-232">HTTPS Port</span></span>

<span data-ttu-id="f92a6-233">Establezca puerto de redirección HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f92a6-233">Set the HTTPS redirect port.</span></span> <span data-ttu-id="f92a6-234">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="f92a6-234">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="f92a6-235">**Clave**: https_port **Tipo**: *cadena*
**Valor predeterminado**: no se ha establecido ningún valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="f92a6-235">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="f92a6-236">**Establecer mediante**: `UseSetting`
**Variable de entorno**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="f92a6-236">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="f92a6-237">Ensamblados de exclusión de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="f92a6-237">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="f92a6-238">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="f92a6-238">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="f92a6-239">**Clave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="f92a6-239">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="f92a6-240">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-240">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-241">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="f92a6-241">**Default**: Empty string</span></span>  
<span data-ttu-id="f92a6-242">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f92a6-242">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f92a6-243">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="f92a6-243">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="f92a6-244">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="f92a6-244">Prefer Hosting URLs</span></span>

<span data-ttu-id="f92a6-245">Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-245">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="f92a6-246">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="f92a6-246">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="f92a6-247">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="f92a6-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f92a6-248">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="f92a6-248">**Default**: true</span></span>  
<span data-ttu-id="f92a6-249">**Establecer mediante**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="f92a6-249">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="f92a6-250">**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="f92a6-250">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="f92a6-251">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="f92a6-251">Prevent Hosting Startup</span></span>

<span data-ttu-id="f92a6-252">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-252">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="f92a6-253">Para más información, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-253">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="f92a6-254">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="f92a6-254">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="f92a6-255">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="f92a6-255">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f92a6-256">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="f92a6-256">**Default**: false</span></span>  
<span data-ttu-id="f92a6-257">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f92a6-257">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f92a6-258">**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="f92a6-258">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="f92a6-259">Direcciones URL de servidor</span><span class="sxs-lookup"><span data-stu-id="f92a6-259">Server URLs</span></span>

<span data-ttu-id="f92a6-260">Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f92a6-260">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="f92a6-261">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="f92a6-261">**Key**: urls</span></span>  
<span data-ttu-id="f92a6-262">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-262">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-263">**Predeterminado**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="f92a6-263">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="f92a6-264">**Establecer mediante**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="f92a6-264">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="f92a6-265">**Variable de entorno**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="f92a6-265">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="f92a6-266">Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="f92a6-266">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="f92a6-267">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-267">For example, `http://localhost:123`.</span></span> <span data-ttu-id="f92a6-268">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="f92a6-268">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="f92a6-269">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="f92a6-269">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="f92a6-270">Los formatos admitidos varían de un servidor a otro.</span><span class="sxs-lookup"><span data-stu-id="f92a6-270">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="f92a6-271">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="f92a6-271">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="f92a6-272">Para más información, consulte <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-272">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="f92a6-273">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="f92a6-273">Shutdown Timeout</span></span>

<span data-ttu-id="f92a6-274">Especifica la cantidad de tiempo que se espera hasta el cierre del host web.</span><span class="sxs-lookup"><span data-stu-id="f92a6-274">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="f92a6-275">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="f92a6-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="f92a6-276">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="f92a6-276">**Type**: *int*</span></span>  
<span data-ttu-id="f92a6-277">**Valor predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="f92a6-277">**Default**: 5</span></span>  
<span data-ttu-id="f92a6-278">**Establecer mediante**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="f92a6-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="f92a6-279">**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="f92a6-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="f92a6-280">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) toma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="f92a6-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="f92a6-281">Durante el período de tiempo de espera, el hospedaje:</span><span class="sxs-lookup"><span data-stu-id="f92a6-281">During the timeout period, hosting:</span></span>

* <span data-ttu-id="f92a6-282">Activa [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="f92a6-282">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="f92a6-283">Trata de detener los servicios hospedados y registra cualquier error que se produzca en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="f92a6-283">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="f92a6-284">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-284">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="f92a6-285">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="f92a6-285">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="f92a6-286">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="f92a6-286">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="f92a6-287">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="f92a6-287">Startup Assembly</span></span>

<span data-ttu-id="f92a6-288">Determina el ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-288">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="f92a6-289">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="f92a6-289">**Key**: startupAssembly</span></span>  
<span data-ttu-id="f92a6-290">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-290">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-291">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f92a6-291">**Default**: The app's assembly</span></span>  
<span data-ttu-id="f92a6-292">**Establecer mediante**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="f92a6-292">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="f92a6-293">**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="f92a6-293">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="f92a6-294">Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="f92a6-294">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="f92a6-295">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="f92a6-295">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="f92a6-296">Raíz web</span><span class="sxs-lookup"><span data-stu-id="f92a6-296">Web Root</span></span>

<span data-ttu-id="f92a6-297">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-297">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="f92a6-298">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="f92a6-298">**Key**: webroot</span></span>  
<span data-ttu-id="f92a6-299">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="f92a6-299">**Type**: *string*</span></span>  
<span data-ttu-id="f92a6-300">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="f92a6-300">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="f92a6-301">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="f92a6-301">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="f92a6-302">**Establecer mediante**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="f92a6-302">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="f92a6-303">**Variable de entorno**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="f92a6-303">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="f92a6-304">Invalidación de la configuración</span><span class="sxs-lookup"><span data-stu-id="f92a6-304">Override configuration</span></span>

<span data-ttu-id="f92a6-305">Use [Configuración](xref:fundamentals/configuration/index) para configurar el host web.</span><span class="sxs-lookup"><span data-stu-id="f92a6-305">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="f92a6-306">En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f92a6-306">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="f92a6-307">Cualquier configuración que se cargue del archivo *hostsettings.json* se puede reemplazar por argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f92a6-307">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="f92a6-308">La configuración generada (en `config`) se usa para configurar el host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="f92a6-308">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="f92a6-309">La configuración de `IWebHostBuilder` se agrega a la de la aplicación, pero lo contrario no es aplicable: `ConfigureAppConfiguration` no afecta a la configuración de `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-309">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="f92a6-310">Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="f92a6-310">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="f92a6-311">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f92a6-311">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="f92a6-312">El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-312">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="f92a6-313">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="f92a6-313">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="f92a6-314">El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="f92a6-314">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="f92a6-315">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="f92a6-315">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="f92a6-316">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="f92a6-316">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="f92a6-317">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="f92a6-317">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="f92a6-318">`UseConfiguration` solo copia las claves del elemento `IConfiguration` proporcionado a la configuración del generador de host.</span><span class="sxs-lookup"><span data-stu-id="f92a6-318">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="f92a6-319">Por consiguiente, el hecho de configurar `reloadOnChange: true` para los archivos de configuración XML, JSON e INI no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="f92a6-319">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="f92a6-320">Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="f92a6-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="f92a6-321">El argumento de línea de comandos reemplaza el valor `urls` del archivo *hostsettings.json*, y el servidor efectúa la escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="f92a6-321">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="f92a6-322">Administración del host</span><span class="sxs-lookup"><span data-stu-id="f92a6-322">Manage the host</span></span>

<span data-ttu-id="f92a6-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="f92a6-323">**Run**</span></span>

<span data-ttu-id="f92a6-324">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="f92a6-324">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="f92a6-325">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="f92a6-325">**Start**</span></span>

<span data-ttu-id="f92a6-326">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="f92a6-327">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="f92a6-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="f92a6-328">La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático.</span><span class="sxs-lookup"><span data-stu-id="f92a6-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="f92a6-329">Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="f92a6-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="f92a6-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="f92a6-331">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="f92a6-332">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="f92a6-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="f92a6-333">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f92a6-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f92a6-334">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="f92a6-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f92a6-335">**Start(url de cadena, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="f92a6-336">Start con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="f92a6-337">Produce el mismo resultado que **Start(RequestDelegate app)** , excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="f92a6-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="f92a6-339">Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="f92a6-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="f92a6-340">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f92a6-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="f92a6-341">Request</span><span class="sxs-lookup"><span data-stu-id="f92a6-341">Request</span></span>                                    | <span data-ttu-id="f92a6-342">Respuesta</span><span class="sxs-lookup"><span data-stu-id="f92a6-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="f92a6-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="f92a6-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="f92a6-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="f92a6-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="f92a6-345">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="f92a6-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="f92a6-346">Produce una excepción con la cadena "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="f92a6-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="f92a6-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="f92a6-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="f92a6-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="f92a6-348">Hello World!</span></span>                             |

<span data-ttu-id="f92a6-349">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f92a6-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f92a6-350">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="f92a6-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f92a6-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="f92a6-352">Use una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="f92a6-353">Produce el mismo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)** , salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-353">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="f92a6-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="f92a6-355">Proporciona un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="f92a6-356">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="f92a6-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="f92a6-357">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f92a6-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f92a6-358">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="f92a6-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f92a6-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="f92a6-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="f92a6-360">Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="f92a6-361">Produce el mismo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)** , salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-361">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="f92a6-362">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="f92a6-362">IHostingEnvironment interface</span></span>

<span data-ttu-id="f92a6-363">La [interfaz IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-363">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="f92a6-364">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="f92a6-364">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="f92a6-365">Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#environment-based-startup-class-and-methods) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="f92a6-365">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="f92a6-366">Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f92a6-366">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="f92a6-367">Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-367">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="f92a6-368">Para más información, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f92a6-368">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="f92a6-369">El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="f92a6-369">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="f92a6-370">`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/write) personalizado:</span><span class="sxs-lookup"><span data-stu-id="f92a6-370">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="f92a6-371">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="f92a6-371">IApplicationLifetime interface</span></span>

<span data-ttu-id="f92a6-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="f92a6-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="f92a6-373">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="f92a6-373">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="f92a6-374">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="f92a6-374">Cancellation Token</span></span>    | <span data-ttu-id="f92a6-375">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="f92a6-375">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="f92a6-376">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="f92a6-376">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="f92a6-377">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="f92a6-377">The host has fully started.</span></span> |
| [<span data-ttu-id="f92a6-378">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="f92a6-378">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="f92a6-379">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="f92a6-379">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="f92a6-380">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f92a6-380">All requests should be processed.</span></span> <span data-ttu-id="f92a6-381">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="f92a6-381">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="f92a6-382">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="f92a6-382">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="f92a6-383">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="f92a6-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="f92a6-384">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f92a6-384">Requests may still be processing.</span></span> <span data-ttu-id="f92a6-385">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="f92a6-385">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="f92a6-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f92a6-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="f92a6-387">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="f92a6-387">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="f92a6-388">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="f92a6-388">Scope validation</span></span>

<span data-ttu-id="f92a6-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f92a6-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="f92a6-390">Cuando `ValidateScopes` está establecido en `true`, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f92a6-390">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="f92a6-391">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="f92a6-391">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="f92a6-392">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="f92a6-392">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="f92a6-393">El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="f92a6-393">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="f92a6-394">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="f92a6-394">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="f92a6-395">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="f92a6-395">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="f92a6-396">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="f92a6-396">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="f92a6-397">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="f92a6-397">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="f92a6-398">Para validar los ámbitos siempre, incluso en el entorno de producción, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) en el generador de hosts:</span><span class="sxs-lookup"><span data-stu-id="f92a6-398">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="f92a6-399">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f92a6-399">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
