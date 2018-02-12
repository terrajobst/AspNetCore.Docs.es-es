---
title: Hospedaje en ASP.NET Core
author: guardrex
description: "Obtenga información sobre el host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="26ab3-103">Hospedaje en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26ab3-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="26ab3-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26ab3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="26ab3-105">Las aplicaciones de ASP.NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="26ab3-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="26ab3-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="26ab3-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="26ab3-108">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="26ab3-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="26ab3-110">Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="26ab3-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="26ab3-111">Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="26ab3-112">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="26ab3-113">Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="26ab3-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="26ab3-114">`CreateDefaultBuilder` realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="26ab3-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="26ab3-115">Configura [Kestrel](servers/kestrel.md) como el servidor web.</span><span class="sxs-lookup"><span data-stu-id="26ab3-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="26ab3-116">Para conocer las opciones predeterminadas de Kestrel, consulte [la sección Opciones de Kestrel de la implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="26ab3-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="26ab3-117">Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="26ab3-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="26ab3-118">Carga configuración opcional de:</span><span class="sxs-lookup"><span data-stu-id="26ab3-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="26ab3-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="26ab3-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="26ab3-121">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="26ab3-122">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="26ab3-122">Environment variables.</span></span>
  * <span data-ttu-id="26ab3-123">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="26ab3-123">Command-line arguments.</span></span>
* <span data-ttu-id="26ab3-124">Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="26ab3-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="26ab3-125">El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="26ab3-126">Cuando se ejecuta detrás de IIS, permite la [integración con IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="26ab3-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="26ab3-127">Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="26ab3-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="26ab3-128">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="26ab3-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="26ab3-129">También configura la aplicación para que [capture errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="26ab3-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="26ab3-130">Para conocer las opciones predeterminadas de IIS, consulte [la sección Opciones de IIS de Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="26ab3-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="26ab3-131">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="26ab3-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="26ab3-132">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="26ab3-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="26ab3-133">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="26ab3-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="26ab3-134">Para más información sobre la configuración de la aplicación, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="26ab3-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="26ab3-135">Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="26ab3-136">Para más información, vea la pestaña ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-138">Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="26ab3-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="26ab3-139">La creación de un host se suele realizar en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="26ab3-140">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="26ab3-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="26ab3-141">`WebHostBuilder` requiere un [servidor que implemente IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="26ab3-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="26ab3-142">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET Core 2.0, HTTP.sys se llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="26ab3-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="26ab3-143">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="26ab3-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="26ab3-144">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="26ab3-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="26ab3-145">La raíz de contenido predeterminada se obtiene para `UseContentRoot` mediante [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="26ab3-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="26ab3-146">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="26ab3-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="26ab3-147">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="26ab3-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="26ab3-148">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="26ab3-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="26ab3-149">A diferencia de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` no configura un *servidor*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="26ab3-150">`UseIISIntegration` configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="26ab3-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="26ab3-151">Para utilizar IIS con ASP.NET Core, deben especificarse `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="26ab3-152">`UseIISIntegration` solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="26ab3-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="26ab3-153">Para obtener más información, consulte [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="26ab3-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="26ab3-154">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de la canalización de solicitudes de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="26ab3-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

---

<span data-ttu-id="26ab3-155">Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="26ab3-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="26ab3-156">Si se especifica una clase `Startup`, debe definir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="26ab3-157">Para obtener más información, vea [Application Startup in ASP.NET Core](startup.md) (Inicio de la aplicación en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="26ab3-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="26ab3-158">Varias llamadas a `ConfigureServices` se anexan entre sí.</span><span class="sxs-lookup"><span data-stu-id="26ab3-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="26ab3-159">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="26ab3-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="26ab3-160">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="26ab3-160">Host configuration values</span></span>

<span data-ttu-id="26ab3-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="26ab3-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="26ab3-162">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="26ab3-163">Por ejemplo: `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="26ab3-164">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="26ab3-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="26ab3-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="26ab3-166">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="26ab3-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="26ab3-167">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="26ab3-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="26ab3-168">Para obtener más información, consulte [Invalidación de la configuración](#overriding-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="26ab3-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="26ab3-169">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="26ab3-169">Capture Startup Errors</span></span>

<span data-ttu-id="26ab3-170">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="26ab3-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="26ab3-171">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="26ab3-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="26ab3-172">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="26ab3-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="26ab3-173">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="26ab3-174">**Establecer mediante**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="26ab3-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="26ab3-175">**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="26ab3-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="26ab3-176">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="26ab3-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="26ab3-177">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="26ab3-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="26ab3-180">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="26ab3-180">Content Root</span></span>

<span data-ttu-id="26ab3-181">Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="26ab3-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="26ab3-182">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="26ab3-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="26ab3-183">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-183">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-184">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="26ab3-185">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="26ab3-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="26ab3-186">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="26ab3-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="26ab3-187">La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="26ab3-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="26ab3-188">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="26ab3-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="26ab3-191">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="26ab3-191">Detailed Errors</span></span>

<span data-ttu-id="26ab3-192">Determina si se deben capturar los errores detallados.</span><span class="sxs-lookup"><span data-stu-id="26ab3-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="26ab3-193">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="26ab3-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="26ab3-194">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="26ab3-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="26ab3-195">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="26ab3-195">**Default**: false</span></span>  
<span data-ttu-id="26ab3-196">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="26ab3-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="26ab3-197">**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="26ab3-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="26ab3-198">Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="26ab3-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="26ab3-201">Entorno</span><span class="sxs-lookup"><span data-stu-id="26ab3-201">Environment</span></span>

<span data-ttu-id="26ab3-202">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-202">Sets the app's environment.</span></span>

<span data-ttu-id="26ab3-203">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="26ab3-203">**Key**: environment</span></span>  
<span data-ttu-id="26ab3-204">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-204">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-205">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="26ab3-205">**Default**: Production</span></span>  
<span data-ttu-id="26ab3-206">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="26ab3-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="26ab3-207">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="26ab3-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="26ab3-208">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="26ab3-208">The environment can be set to any value.</span></span> <span data-ttu-id="26ab3-209">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="26ab3-210">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="26ab3-210">Values aren't case sensitive.</span></span> <span data-ttu-id="26ab3-211">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="26ab3-212">Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="26ab3-213">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="26ab3-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="26ab3-216">Ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="26ab3-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="26ab3-217">Establece los ensamblados de inicio de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="26ab3-218">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="26ab3-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="26ab3-219">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-219">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-220">**Valor predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="26ab3-220">**Default**: Empty string</span></span>  
<span data-ttu-id="26ab3-221">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="26ab3-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="26ab3-222">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="26ab3-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="26ab3-223">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="26ab3-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="26ab3-224">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="26ab3-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="26ab3-225">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="26ab3-226">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="26ab3-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-229">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="26ab3-230">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="26ab3-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="26ab3-231">Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="26ab3-232">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="26ab3-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="26ab3-233">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="26ab3-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="26ab3-234">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="26ab3-234">**Default**: true</span></span>  
<span data-ttu-id="26ab3-235">**Establecer mediante**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="26ab3-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="26ab3-236">**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="26ab3-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="26ab3-237">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="26ab3-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-240">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="26ab3-241">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="26ab3-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="26ab3-242">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="26ab3-243">Consulte [Incorporación de características de aplicación desde un ensamblado externo mediante IHostingStartup](xref:host-and-deploy/ihostingstartup) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="26ab3-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="26ab3-244">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="26ab3-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="26ab3-245">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="26ab3-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="26ab3-246">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="26ab3-246">**Default**: false</span></span>  
<span data-ttu-id="26ab3-247">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="26ab3-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="26ab3-248">**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="26ab3-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="26ab3-249">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="26ab3-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-252">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="26ab3-253">Direcciones URL de servidor</span><span class="sxs-lookup"><span data-stu-id="26ab3-253">Server URLs</span></span>

<span data-ttu-id="26ab3-254">Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="26ab3-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="26ab3-255">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="26ab3-255">**Key**: urls</span></span>  
<span data-ttu-id="26ab3-256">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-256">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-257">**Valor predeterminado**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="26ab3-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="26ab3-258">**Establecer mediante**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="26ab3-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="26ab3-259">**Variable de entorno**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="26ab3-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="26ab3-260">Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="26ab3-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="26ab3-261">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="26ab3-262">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="26ab3-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="26ab3-263">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="26ab3-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="26ab3-264">Los formatos admitidos varían en función del servidor.</span><span class="sxs-lookup"><span data-stu-id="26ab3-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="26ab3-266">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="26ab3-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="26ab3-267">Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="26ab3-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="26ab3-269">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="26ab3-269">Shutdown Timeout</span></span>

<span data-ttu-id="26ab3-270">Especifica la cantidad de tiempo que se espera hasta que se apague el host de web.</span><span class="sxs-lookup"><span data-stu-id="26ab3-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="26ab3-271">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="26ab3-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="26ab3-272">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="26ab3-272">**Type**: *int*</span></span>  
<span data-ttu-id="26ab3-273">**Valor predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="26ab3-273">**Default**: 5</span></span>  
<span data-ttu-id="26ab3-274">**Establecer mediante**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="26ab3-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="26ab3-275">**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="26ab3-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="26ab3-276">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión `UseShutdownTimeout` toma `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="26ab3-277">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="26ab3-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-280">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="26ab3-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="26ab3-281">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="26ab3-281">Startup Assembly</span></span>

<span data-ttu-id="26ab3-282">Determina el ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="26ab3-283">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="26ab3-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="26ab3-284">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-284">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-285">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="26ab3-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="26ab3-286">**Establecer mediante**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="26ab3-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="26ab3-287">**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="26ab3-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="26ab3-288">Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="26ab3-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="26ab3-289">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="26ab3-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="26ab3-292">Raíz web</span><span class="sxs-lookup"><span data-stu-id="26ab3-292">Web Root</span></span>

<span data-ttu-id="26ab3-293">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="26ab3-294">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="26ab3-294">**Key**: webroot</span></span>  
<span data-ttu-id="26ab3-295">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="26ab3-295">**Type**: *string*</span></span>  
<span data-ttu-id="26ab3-296">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="26ab3-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="26ab3-297">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="26ab3-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="26ab3-298">**Establecer mediante**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="26ab3-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="26ab3-299">**Variable de entorno**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="26ab3-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="26ab3-302">Invalidación de la configuración</span><span class="sxs-lookup"><span data-stu-id="26ab3-302">Overriding configuration</span></span>

<span data-ttu-id="26ab3-303">Use [Configuración](xref:fundamentals/configuration/index) para configurar el host.</span><span class="sxs-lookup"><span data-stu-id="26ab3-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="26ab3-304">En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="26ab3-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="26ab3-305">Cualquier configuración que se carga desde el archivo *hosting.json* puede reemplazarse por argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="26ab3-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="26ab3-306">La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="26ab3-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="26ab3-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="26ab3-309">Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="26ab3-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="26ab3-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="26ab3-312">Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="26ab3-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> <span data-ttu-id="26ab3-313">El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="26ab3-314">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="26ab3-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="26ab3-315">El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="26ab3-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="26ab3-316">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="26ab3-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="26ab3-317">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="26ab3-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="26ab3-318">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="26ab3-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="26ab3-319">Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="26ab3-320">El argumento de línea de comandos reemplaza el valor `urls` del archivo *hosting.json*, y el servidor escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="26ab3-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="26ab3-321">Inicio del host</span><span class="sxs-lookup"><span data-stu-id="26ab3-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="26ab3-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="26ab3-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="26ab3-323">**Run**</span></span>

<span data-ttu-id="26ab3-324">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="26ab3-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="26ab3-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="26ab3-325">**Start**</span></span>

<span data-ttu-id="26ab3-326">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="26ab3-327">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="26ab3-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="26ab3-328">La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático.</span><span class="sxs-lookup"><span data-stu-id="26ab3-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="26ab3-329">Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="26ab3-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="26ab3-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="26ab3-331">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="26ab3-332">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="26ab3-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="26ab3-333">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="26ab3-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="26ab3-334">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="26ab3-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="26ab3-335">**Start(url de cadena, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="26ab3-336">Start con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="26ab3-337">Produce el mismo resultado que **Start(RequestDelegate app)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="26ab3-338">**Start(acción<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="26ab3-339">Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="26ab3-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="26ab3-340">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="26ab3-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="26ab3-341">Solicitud</span><span class="sxs-lookup"><span data-stu-id="26ab3-341">Request</span></span>                                    | <span data-ttu-id="26ab3-342">Respuesta</span><span class="sxs-lookup"><span data-stu-id="26ab3-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="26ab3-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="26ab3-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="26ab3-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="26ab3-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="26ab3-345">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="26ab3-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="26ab3-346">Produce una excepción con la cadena "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="26ab3-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="26ab3-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="26ab3-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="26ab3-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="26ab3-348">Hello World!</span></span>                             |

<span data-ttu-id="26ab3-349">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="26ab3-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="26ab3-350">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="26ab3-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="26ab3-351">**Start(url de cadena, acción<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="26ab3-352">Use una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="26ab3-353">Produce el mismo resultado que **Start(acción<IRouteBuilder> routeBuilder)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="26ab3-354">**StartWith(acción<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="26ab3-355">Proporciona un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="26ab3-356">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="26ab3-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="26ab3-357">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="26ab3-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="26ab3-358">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="26ab3-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="26ab3-359">**StartWith(url de cadena, acción<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="26ab3-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="26ab3-360">Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="26ab3-361">Produce el mismo resultado que **StartWith(acción<IApplicationBuilder> app)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="26ab3-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="26ab3-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="26ab3-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="26ab3-363">**Run**</span></span>

<span data-ttu-id="26ab3-364">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="26ab3-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="26ab3-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="26ab3-365">**Start**</span></span>

<span data-ttu-id="26ab3-366">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="26ab3-367">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="26ab3-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="26ab3-368">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="26ab3-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="26ab3-369">La [interfaz IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ab3-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="26ab3-370">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="26ab3-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="26ab3-371">Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="26ab3-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="26ab3-372">Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="26ab3-373">Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="26ab3-374">Para más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="26ab3-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="26ab3-375">El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="26ab3-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="26ab3-376">`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="26ab3-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="26ab3-377">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="26ab3-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="26ab3-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="26ab3-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="26ab3-379">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="26ab3-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="26ab3-380">También hay un método `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="26ab3-381">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="26ab3-381">Cancellation Token</span></span>    | <span data-ttu-id="26ab3-382">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="26ab3-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="26ab3-383">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="26ab3-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="26ab3-384">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="26ab3-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="26ab3-385">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="26ab3-385">Requests may still be processing.</span></span> <span data-ttu-id="26ab3-386">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="26ab3-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="26ab3-387">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="26ab3-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="26ab3-388">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="26ab3-388">All requests should be processed.</span></span> <span data-ttu-id="26ab3-389">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="26ab3-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="26ab3-390">Método</span><span class="sxs-lookup"><span data-stu-id="26ab3-390">Method</span></span>            | <span data-ttu-id="26ab3-391">Acción</span><span class="sxs-lookup"><span data-stu-id="26ab3-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="26ab3-392">Solicita la terminación de la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="26ab3-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="26ab3-393">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="26ab3-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="26ab3-394">**Se aplica únicamente a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="26ab3-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="26ab3-395">Se puede crear un host mediante la inserción directa de `IStartup` en el contenedor de inserción de dependencias en lugar de llamar a `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="26ab3-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="26ab3-396">Si el host se crea de esta forma, puede producirse el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="26ab3-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="26ab3-397">Esto ocurre porque [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="26ab3-398">Si la aplicación inserta manualmente `IStartup` en el contenedor de inserción de dependencias, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="26ab3-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="26ab3-399">O bien, agregue un `Configure` ficticio a `WebHostBuilder`, que establece `applicationName`(`ApplicationKey`) automáticamente:</span><span class="sxs-lookup"><span data-stu-id="26ab3-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="26ab3-400">**NOTA**: Esto solo es necesario con la versión ASP.NET Core 2.0 y únicamente cuando la aplicación no llama a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="26ab3-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="26ab3-401">Para obtener más información, vea [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario) y el [ejemplo StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="26ab3-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26ab3-402">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="26ab3-402">Additional resources</span></span>

* [<span data-ttu-id="26ab3-403">Hospedaje en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="26ab3-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="26ab3-404">Hospedaje en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="26ab3-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="26ab3-405">Hospedaje en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="26ab3-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="26ab3-406">Hospedaje en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="26ab3-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
