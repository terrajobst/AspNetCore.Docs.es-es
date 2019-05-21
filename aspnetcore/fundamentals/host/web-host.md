---
title: Host web de ASP.NET Core
author: guardrex
description: Obtenga información sobre el host web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 48f3b664d901bdfb27cdf9e798fa60c0587d1def
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610283"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="1e215-103">Host web de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e215-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="1e215-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1e215-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1e215-105">Las aplicaciones de ASP.NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="1e215-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="1e215-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1e215-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="1e215-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="1e215-108">El host también puede configurar el registro, la inserción de dependencias y la configuración.</span><span class="sxs-lookup"><span data-stu-id="1e215-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1e215-109">Para obtener la versión 1.1 de este tema, descargue [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf) [Host de ASP.NET Core Web (versión 1.1, PDF)].</span><span class="sxs-lookup"><span data-stu-id="1e215-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="1e215-110">En este artículo se describe el host web de ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), que sirve para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="1e215-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="1e215-111">Para obtener información sobre el host genérico de .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), vea <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="1e215-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="1e215-112">En este artículo se describe el host web de ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="1e215-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="1e215-113">En ASP.NET Core 3.0, el host genérico reemplaza al host web.</span><span class="sxs-lookup"><span data-stu-id="1e215-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="1e215-114">Para obtener más información, vea [El host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="1e215-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="1e215-115">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="1e215-115">Set up a host</span></span>

<span data-ttu-id="1e215-116">Cree un host con una instancia de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1e215-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="1e215-117">Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="1e215-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="1e215-118">El nombre del método del generador, `CreateWebHostBuilder`, es un nombre especial que identifica el método de generador para los componentes externos, como [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="1e215-118">The builder method name, `CreateWebHostBuilder`, is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="1e215-119">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1e215-119">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1e215-120">Una aplicación típica llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="1e215-120">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="1e215-121">`CreateDefaultBuilder` realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e215-121">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="1e215-122">Configura el servidor [Kestrel](xref:fundamentals/servers/kestrel) como servidor web por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-122">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="1e215-123">Para conocer las opciones predeterminadas del servidor Kestrel, consulte <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="1e215-123">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="1e215-124">Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1e215-124">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="1e215-125">Carga la [configuración de host](#host-configuration-values) de:</span><span class="sxs-lookup"><span data-stu-id="1e215-125">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="1e215-126">Variables de entorno con el prefijo `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="1e215-126">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="1e215-127">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1e215-127">Command-line arguments.</span></span>
* <span data-ttu-id="1e215-128">Carga la configuración de la aplicación en el siguiente orden:</span><span class="sxs-lookup"><span data-stu-id="1e215-128">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="1e215-129">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1e215-129">*appsettings.json*.</span></span>
  * <span data-ttu-id="1e215-130">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1e215-130">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1e215-131">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="1e215-131">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1e215-132">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="1e215-132">Environment variables.</span></span>
  * <span data-ttu-id="1e215-133">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1e215-133">Command-line arguments.</span></span>
* <span data-ttu-id="1e215-134">Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="1e215-134">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="1e215-135">El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1e215-135">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="1e215-136">Cuando se ejecuta detrás de IIS con el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` habilita la [integración de IIS](xref:host-and-deploy/iis/index), que configura la dirección base y el puerto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-136">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="1e215-137">La integración de IIS también configura la aplicación para que [capture errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="1e215-137">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="1e215-138">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="1e215-138">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="1e215-139">Establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1e215-139">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="1e215-140">Para más información, vea [Validación del ámbito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="1e215-140">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="1e215-141">La configuración definida en `CreateDefaultBuilder` se puede reemplazar y aumentar mediante [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) y otros métodos y métodos de extensión de [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1e215-141">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="1e215-142">A continuación, se presentan algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="1e215-142">A few examples follow:</span></span>

* <span data-ttu-id="1e215-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) se usa para especificar `IConfiguration` adicionales para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="1e215-144">La siguiente llamada `ConfigureAppConfiguration` agrega un delegado para incluir la configuración de la aplicación en el archivo *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="1e215-144">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="1e215-145">Es posible llamar a `ConfigureAppConfiguration` varias veces.</span><span class="sxs-lookup"><span data-stu-id="1e215-145">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="1e215-146">Tenga en cuenta que esta configuración no se aplica al host (por ejemplo, direcciones URL de servidor o entorno).</span><span class="sxs-lookup"><span data-stu-id="1e215-146">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="1e215-147">Vea la sección [Valores de configuración de host](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="1e215-147">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="1e215-148">La siguiente llamada `ConfigureLogging` agrega un delegado para configurar el nivel de registro mínimo ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) en [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="1e215-148">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="1e215-149">Esta configuración reemplaza la configuración de *appsettings.Development.JSON* (`LogLevel.Debug`) y *appsettings.Production.JSON* (`LogLevel.Error`) configurada mediante `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1e215-149">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1e215-150">Es posible llamar a `ConfigureLogging` varias veces.</span><span class="sxs-lookup"><span data-stu-id="1e215-150">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="1e215-151">La siguiente llamada a `ConfigureKestrel` reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1e215-151">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="1e215-152">La siguiente llamada a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) reemplaza el valor predeterminado de [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) de 30 000 000 bytes establecido al configurar Kestrel mediante `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1e215-152">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="1e215-153">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="1e215-153">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1e215-154">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="1e215-154">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="1e215-155">Este es el valor predeterminado usado en [Visual Studio](https://visualstudio.microsoft.com) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1e215-155">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1e215-156">Para obtener más información sobre la configuración de la aplicación, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1e215-156">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="1e215-157">Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="1e215-157">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="1e215-158">Para más información, vea la pestaña ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1e215-158">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="1e215-159">Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="1e215-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="1e215-160">Si se especifica una clase `Startup`, debe definir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="1e215-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="1e215-161">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="1e215-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="1e215-162">Varias llamadas a `ConfigureServices` se anexan entre sí.</span><span class="sxs-lookup"><span data-stu-id="1e215-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="1e215-163">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="1e215-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1e215-164">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="1e215-164">Host configuration values</span></span>

<span data-ttu-id="1e215-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="1e215-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="1e215-166">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="1e215-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="1e215-167">Por ejemplo: `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1e215-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="1e215-168">Extensiones como [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) y [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (vea la sección [Invalidación de la configuración](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="1e215-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="1e215-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="1e215-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="1e215-170">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="1e215-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="1e215-171">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="1e215-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="1e215-172">Para obtener más información, consulte [Invalidación de la configuración](#override-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="1e215-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="1e215-173">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="1e215-173">Application Key (Name)</span></span>

<span data-ttu-id="1e215-174">La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece automáticamente cuando se llama a [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) o [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) durante la construcción del host.</span><span class="sxs-lookup"><span data-stu-id="1e215-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="1e215-175">El valor se establece en el nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="1e215-176">Para establecer el valor explícitamente, use [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="1e215-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="1e215-177">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="1e215-177">**Key**: applicationName</span></span>  
<span data-ttu-id="1e215-178">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-178">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-179">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="1e215-180">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1e215-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1e215-181">**Variable de entorno**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="1e215-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="1e215-182">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="1e215-182">Capture Startup Errors</span></span>

<span data-ttu-id="1e215-183">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="1e215-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="1e215-184">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1e215-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1e215-185">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="1e215-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1e215-186">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="1e215-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1e215-187">**Establecer mediante**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="1e215-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="1e215-188">**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="1e215-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="1e215-189">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="1e215-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1e215-190">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="1e215-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="1e215-191">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="1e215-191">Content Root</span></span>

<span data-ttu-id="1e215-192">Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="1e215-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="1e215-193">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1e215-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="1e215-194">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-194">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-195">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1e215-196">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1e215-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1e215-197">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="1e215-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="1e215-198">La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="1e215-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="1e215-199">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="1e215-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="1e215-200">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="1e215-200">Detailed Errors</span></span>

<span data-ttu-id="1e215-201">Determina si se deben capturar los errores detallados.</span><span class="sxs-lookup"><span data-stu-id="1e215-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="1e215-202">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="1e215-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1e215-203">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="1e215-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1e215-204">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="1e215-204">**Default**: false</span></span>  
<span data-ttu-id="1e215-205">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1e215-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1e215-206">**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="1e215-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="1e215-207">Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="1e215-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="1e215-208">Entorno</span><span class="sxs-lookup"><span data-stu-id="1e215-208">Environment</span></span>

<span data-ttu-id="1e215-209">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-209">Sets the app's environment.</span></span>

<span data-ttu-id="1e215-210">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="1e215-210">**Key**: environment</span></span>  
<span data-ttu-id="1e215-211">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-211">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-212">**Valor predeterminado**: Producción</span><span class="sxs-lookup"><span data-stu-id="1e215-212">**Default**: Production</span></span>  
<span data-ttu-id="1e215-213">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1e215-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1e215-214">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1e215-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="1e215-215">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="1e215-215">The environment can be set to any value.</span></span> <span data-ttu-id="1e215-216">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="1e215-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1e215-217">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1e215-217">Values aren't case sensitive.</span></span> <span data-ttu-id="1e215-218">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1e215-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1e215-219">Cuando se usa [Visual Studio](https://visualstudio.microsoft.com), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1e215-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="1e215-220">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1e215-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="1e215-221">Ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1e215-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="1e215-222">Establece los ensamblados de inicio de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="1e215-223">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1e215-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1e215-224">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-224">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-225">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="1e215-225">**Default**: Empty string</span></span>  
<span data-ttu-id="1e215-226">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1e215-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1e215-227">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1e215-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="1e215-228">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="1e215-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="1e215-229">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1e215-230">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="1e215-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="1e215-231">Puerto HTTPS</span><span class="sxs-lookup"><span data-stu-id="1e215-231">HTTPS Port</span></span>

<span data-ttu-id="1e215-232">Establezca puerto de redirección HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1e215-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="1e215-233">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="1e215-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="1e215-234">**Clave**: https_port **Tipo**: *cadena*
**Valor predeterminado**: no se ha establecido ningún valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1e215-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="1e215-235">**Establecer mediante**: `UseSetting`
**Variable de entorno**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="1e215-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="1e215-236">Ensamblados de exclusión de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1e215-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="1e215-237">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="1e215-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="1e215-238">**Clave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="1e215-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="1e215-239">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-239">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-240">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="1e215-240">**Default**: Empty string</span></span>  
<span data-ttu-id="1e215-241">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1e215-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1e215-242">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1e215-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="1e215-243">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1e215-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="1e215-244">Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="1e215-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1e215-245">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1e215-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1e215-246">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="1e215-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1e215-247">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="1e215-247">**Default**: true</span></span>  
<span data-ttu-id="1e215-248">**Establecer mediante**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="1e215-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="1e215-249">**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="1e215-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="1e215-250">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1e215-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="1e215-251">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="1e215-252">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1e215-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="1e215-253">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1e215-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1e215-254">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="1e215-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1e215-255">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="1e215-255">**Default**: false</span></span>  
<span data-ttu-id="1e215-256">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1e215-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="1e215-257">**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1e215-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="1e215-258">Direcciones URL de servidor</span><span class="sxs-lookup"><span data-stu-id="1e215-258">Server URLs</span></span>

<span data-ttu-id="1e215-259">Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="1e215-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="1e215-260">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="1e215-260">**Key**: urls</span></span>  
<span data-ttu-id="1e215-261">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-261">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-262">**Predeterminado**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="1e215-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="1e215-263">**Establecer mediante**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="1e215-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="1e215-264">**Variable de entorno**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="1e215-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="1e215-265">Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="1e215-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="1e215-266">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1e215-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1e215-267">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="1e215-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1e215-268">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="1e215-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1e215-269">Los formatos admitidos varían de un servidor a otro.</span><span class="sxs-lookup"><span data-stu-id="1e215-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="1e215-270">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1e215-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1e215-271">Para obtener más información, vea <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1e215-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="1e215-272">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="1e215-272">Shutdown Timeout</span></span>

<span data-ttu-id="1e215-273">Especifica la cantidad de tiempo que se espera hasta el cierre del host web.</span><span class="sxs-lookup"><span data-stu-id="1e215-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="1e215-274">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1e215-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1e215-275">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="1e215-275">**Type**: *int*</span></span>  
<span data-ttu-id="1e215-276">**Valor predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="1e215-276">**Default**: 5</span></span>  
<span data-ttu-id="1e215-277">**Establecer mediante**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="1e215-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="1e215-278">**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="1e215-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="1e215-279">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) toma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="1e215-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="1e215-280">Durante el período de tiempo de espera, el hospedaje:</span><span class="sxs-lookup"><span data-stu-id="1e215-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="1e215-281">Activa [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="1e215-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="1e215-282">Trata de detener los servicios hospedados y registra cualquier error que se produzca en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="1e215-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="1e215-283">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="1e215-284">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="1e215-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="1e215-285">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="1e215-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="1e215-286">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="1e215-286">Startup Assembly</span></span>

<span data-ttu-id="1e215-287">Determina el ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1e215-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1e215-288">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="1e215-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1e215-289">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-289">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-290">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="1e215-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1e215-291">**Establecer mediante**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="1e215-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="1e215-292">**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="1e215-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="1e215-293">Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="1e215-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="1e215-294">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="1e215-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="1e215-295">Raíz web</span><span class="sxs-lookup"><span data-stu-id="1e215-295">Web Root</span></span>

<span data-ttu-id="1e215-296">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="1e215-297">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="1e215-297">**Key**: webroot</span></span>  
<span data-ttu-id="1e215-298">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="1e215-298">**Type**: *string*</span></span>  
<span data-ttu-id="1e215-299">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="1e215-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="1e215-300">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="1e215-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="1e215-301">**Establecer mediante**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="1e215-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="1e215-302">**Variable de entorno**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="1e215-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="1e215-303">Invalidación de la configuración</span><span class="sxs-lookup"><span data-stu-id="1e215-303">Override configuration</span></span>

<span data-ttu-id="1e215-304">Use [Configuración](xref:fundamentals/configuration/index) para configurar el host web.</span><span class="sxs-lookup"><span data-stu-id="1e215-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="1e215-305">En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1e215-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="1e215-306">Cualquier configuración que se cargue del archivo *hostsettings.json* se puede reemplazar por argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1e215-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="1e215-307">La configuración generada (en `config`) se usa para configurar el host con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="1e215-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="1e215-308">La configuración de `IWebHostBuilder` se agrega a la de la aplicación, pero lo contrario no es aplicable: `ConfigureAppConfiguration` no afecta a la configuración de `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1e215-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="1e215-309">Reemplazar primero la configuración proporcionada por `UseUrls` por la de *hostsettings.json* y, luego, por la del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="1e215-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="1e215-310">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1e215-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="1e215-311">El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="1e215-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1e215-312">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="1e215-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="1e215-313">El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="1e215-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="1e215-314">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="1e215-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1e215-315">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="1e215-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1e215-316">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="1e215-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="1e215-317">`UseConfiguration` solo copia las claves del elemento `IConfiguration` proporcionado a la configuración del generador de host.</span><span class="sxs-lookup"><span data-stu-id="1e215-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="1e215-318">Por consiguiente, el hecho de configurar `reloadOnChange: true` para los archivos de configuración XML, JSON e INI no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="1e215-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="1e215-319">Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="1e215-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="1e215-320">El argumento de línea de comandos reemplaza el valor `urls` del archivo *hostsettings.json*, y el servidor efectúa la escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="1e215-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="1e215-321">Administración del host</span><span class="sxs-lookup"><span data-stu-id="1e215-321">Manage the host</span></span>

<span data-ttu-id="1e215-322">**Run**</span><span class="sxs-lookup"><span data-stu-id="1e215-322">**Run**</span></span>

<span data-ttu-id="1e215-323">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="1e215-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1e215-324">**Iniciar**</span><span class="sxs-lookup"><span data-stu-id="1e215-324">**Start**</span></span>

<span data-ttu-id="1e215-325">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="1e215-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1e215-326">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="1e215-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="1e215-327">La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático.</span><span class="sxs-lookup"><span data-stu-id="1e215-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="1e215-328">Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="1e215-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="1e215-329">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1e215-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="1e215-330">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1e215-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1e215-331">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="1e215-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1e215-332">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="1e215-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1e215-333">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="1e215-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1e215-334">**Start(url de cadena, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="1e215-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="1e215-335">Start con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1e215-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1e215-336">Produce el mismo resultado que **Start(RequestDelegate app)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1e215-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="1e215-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1e215-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1e215-338">Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="1e215-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="1e215-339">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1e215-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="1e215-340">Request</span><span class="sxs-lookup"><span data-stu-id="1e215-340">Request</span></span>                                    | <span data-ttu-id="1e215-341">Respuesta</span><span class="sxs-lookup"><span data-stu-id="1e215-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="1e215-342">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="1e215-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="1e215-343">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="1e215-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="1e215-344">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="1e215-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="1e215-345">Produce una excepción con la cadena "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="1e215-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="1e215-346">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="1e215-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="1e215-347">Hello World!</span><span class="sxs-lookup"><span data-stu-id="1e215-347">Hello World!</span></span>                             |

<span data-ttu-id="1e215-348">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="1e215-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1e215-349">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="1e215-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1e215-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1e215-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="1e215-351">Use una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1e215-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="1e215-352">Produce el mismo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1e215-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="1e215-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="1e215-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1e215-354">Proporciona un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1e215-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1e215-355">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="1e215-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1e215-356">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="1e215-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1e215-357">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="1e215-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1e215-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="1e215-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="1e215-359">Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1e215-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="1e215-360">Produce el mismo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1e215-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1e215-361">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="1e215-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="1e215-362">La [interfaz IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="1e215-363">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="1e215-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1e215-364">Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#environment-based-startup-class-and-methods) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="1e215-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="1e215-365">Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1e215-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="1e215-366">Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="1e215-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="1e215-367">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1e215-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1e215-368">El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="1e215-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="1e215-369">`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/write) personalizado:</span><span class="sxs-lookup"><span data-stu-id="1e215-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1e215-370">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="1e215-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="1e215-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="1e215-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="1e215-372">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="1e215-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1e215-373">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="1e215-373">Cancellation Token</span></span>    | <span data-ttu-id="1e215-374">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="1e215-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="1e215-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="1e215-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="1e215-376">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="1e215-376">The host has fully started.</span></span> |
| [<span data-ttu-id="1e215-377">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="1e215-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="1e215-378">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="1e215-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1e215-379">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="1e215-379">All requests should be processed.</span></span> <span data-ttu-id="1e215-380">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="1e215-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="1e215-381">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="1e215-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="1e215-382">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="1e215-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1e215-383">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="1e215-383">Requests may still be processing.</span></span> <span data-ttu-id="1e215-384">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="1e215-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="1e215-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1e215-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="1e215-386">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="1e215-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="1e215-387">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="1e215-387">Scope validation</span></span>

<span data-ttu-id="1e215-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1e215-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="1e215-389">Cuando `ValidateScopes` está establecido en `true`, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1e215-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="1e215-390">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="1e215-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="1e215-391">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="1e215-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="1e215-392">El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="1e215-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="1e215-393">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="1e215-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="1e215-394">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="1e215-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="1e215-395">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="1e215-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="1e215-396">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="1e215-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="1e215-397">Para validar los ámbitos siempre, incluso en el entorno de producción, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) en el generador de hosts:</span><span class="sxs-lookup"><span data-stu-id="1e215-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="1e215-398">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1e215-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
