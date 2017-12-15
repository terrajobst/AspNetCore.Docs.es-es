---
title: Hospedar en ASP.NET Core
author: guardrex
description: "Obtenga información acerca del host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación."
keywords: "Núcleo de ASP.NET, web host, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="4575f-104">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4575f-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="4575f-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4575f-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4575f-106">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4575f-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="4575f-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="4575f-108">Configurar un host</span><span class="sxs-lookup"><span data-stu-id="4575f-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4575f-110">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4575f-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4575f-111">Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="4575f-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="4575f-112">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4575f-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4575f-113">Una típica *Program.cs* llamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="4575f-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="4575f-114">`CreateDefaultBuilder`realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="4575f-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="4575f-115">Configura [Kestrel](servers/kestrel.md) como el servidor web.</span><span class="sxs-lookup"><span data-stu-id="4575f-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="4575f-116">Para las opciones predeterminadas de Kestrel, consulte [el Kestrel opciones de sección de implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="4575f-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="4575f-117">Establece la raíz del contenido en [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4575f-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="4575f-118">Configuración opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="4575f-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="4575f-119">*appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="4575f-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="4575f-120">*appSettings. {Entorno} .json*.</span><span class="sxs-lookup"><span data-stu-id="4575f-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="4575f-121">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el `Development` entorno.</span><span class="sxs-lookup"><span data-stu-id="4575f-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="4575f-122">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="4575f-122">Environment variables.</span></span>
  * <span data-ttu-id="4575f-123">Argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="4575f-123">Command-line arguments.</span></span>
* <span data-ttu-id="4575f-124">Configura [registro](xref:fundamentals/logging/index) de la salida de consola y de depuración a [filtrado del registro](xref:fundamentals/logging/index#log-filtering) las reglas especificadas en una sección de configuración de registro de un *appSettings.JSON que se* o *appsettings. {Entorno} .json* archivo.</span><span class="sxs-lookup"><span data-stu-id="4575f-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="4575f-125">Cuando se ejecuta detrás de IIS, permite [integración con IIS](xref:publishing/iis) mediante la configuración de la ruta de acceso base y el puerto del servidor debe escuchar al usar el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4575f-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="4575f-126">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4575f-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="4575f-127">También configura la aplicación [capturar errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="4575f-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="4575f-128">Para las opciones predeterminadas IIS, consulte [IIS opciones de sección de Host de ASP.NET Core en Windows con IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="4575f-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="4575f-129">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="4575f-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4575f-130">La raíz de contenido de forma predeterminada es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="4575f-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="4575f-131">Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto).</span><span class="sxs-lookup"><span data-stu-id="4575f-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="4575f-132">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4575f-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4575f-133">Vea [configuración en ASP.NET Core](xref:fundamentals/configuration/index) para obtener más información sobre la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="4575f-134">Como alternativa al uso el método estático `CreateDefaultBuilder` (método), crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="4575f-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="4575f-135">Vea la pestaña ASP.NET Core 1.x para más información.</span><span class="sxs-lookup"><span data-stu-id="4575f-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-137">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="4575f-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="4575f-138">Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="4575f-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="4575f-139">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="4575f-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="4575f-140">El siguiente *Program.cs* muestra cómo utilizar `WebHostBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="4575f-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="4575f-141">`WebHostBuilder`requiere un [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4575f-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="4575f-142">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET 2.0 de núcleo, HTTP.sys llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="4575f-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="4575f-143">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4575f-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="4575f-144">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="4575f-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="4575f-145">La raíz de contenido predeterminado proporcionado para `UseContentRoot` es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="4575f-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="4575f-146">Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto).</span><span class="sxs-lookup"><span data-stu-id="4575f-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="4575f-147">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="4575f-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="4575f-148">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="4575f-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="4575f-149">`UseIISIntegration`No configure un *server*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="4575f-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="4575f-150">`UseIISIntegration`configura la ruta de acceso base y el puerto que debe escuchar el servidor cuando se usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="4575f-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="4575f-151">Para usar IIS con ASP.NET Core, debe especificar tanto `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="4575f-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="4575f-152">`UseIISIntegration`solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4575f-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="4575f-153">Para obtener más información, consulte [Introducción a ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) y [referencia de configuración de ASP.NET Core módulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4575f-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="4575f-154">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de canalización de solicitud de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="4575f-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="4575f-155">Al configurar un host, puede proporcionar [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos.</span><span class="sxs-lookup"><span data-stu-id="4575f-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="4575f-156">Si especifica un `Startup` (clase), debe definir un `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="4575f-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="4575f-157">Para obtener más información, consulte [inicio de la aplicación de ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="4575f-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="4575f-158">Varias llamadas a `ConfigureServices` anexar entre sí.</span><span class="sxs-lookup"><span data-stu-id="4575f-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="4575f-159">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` sustituye la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="4575f-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="4575f-160">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="4575f-160">Host configuration values</span></span>

<span data-ttu-id="4575f-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) proporciona métodos para establecer la mayoría de los valores de configuración disponibles para el host, que también se puede establecer directamente con [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="4575f-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="4575f-162">Al establecer un valor con `UseSetting`, el valor se establece como una cadena (entre comillas), independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="4575f-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="4575f-163">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="4575f-163">Capture Startup Errors</span></span>

<span data-ttu-id="4575f-164">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="4575f-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="4575f-165">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="4575f-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="4575f-166">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4575f-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4575f-167">**Valor predeterminado**: valor predeterminado es `false` a menos que la aplicación se ejecuta con Kestrel detrás de IIS, donde el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="4575f-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="4575f-168">**Establecer mediante**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="4575f-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="4575f-169">Cuando `false`, errores durante el resultado de inicio en el host de salir.</span><span class="sxs-lookup"><span data-stu-id="4575f-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="4575f-170">Cuando `true`, el host captura excepciones durante el inicio y se intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="4575f-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="4575f-173">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="4575f-173">Content Root</span></span>

<span data-ttu-id="4575f-174">Esta configuración determina donde ASP.NET Core comienza a buscar archivos de contenido, como las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="4575f-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="4575f-175">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="4575f-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="4575f-176">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-176">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-177">**Predeterminado**: valor predeterminado es la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="4575f-178">**Establecer mediante**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="4575f-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="4575f-179">La raíz de contenido también se usa como la ruta de acceso base para la [configuración Web raíz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="4575f-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="4575f-180">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="4575f-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="4575f-183">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="4575f-183">Detailed Errors</span></span>

<span data-ttu-id="4575f-184">Determina si el registro detallado se deben capturar los errores.</span><span class="sxs-lookup"><span data-stu-id="4575f-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="4575f-185">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="4575f-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="4575f-186">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4575f-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4575f-187">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="4575f-187">**Default**: false</span></span>  
<span data-ttu-id="4575f-188">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4575f-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="4575f-189">Cuando habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="4575f-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="4575f-192">Entorno</span><span class="sxs-lookup"><span data-stu-id="4575f-192">Environment</span></span>

<span data-ttu-id="4575f-193">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-193">Sets the app's environment.</span></span>

<span data-ttu-id="4575f-194">**Clave**: entorno</span><span class="sxs-lookup"><span data-stu-id="4575f-194">**Key**: environment</span></span>  
<span data-ttu-id="4575f-195">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-195">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-196">**Predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="4575f-196">**Default**: Production</span></span>  
<span data-ttu-id="4575f-197">**Establecer mediante**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="4575f-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="4575f-198">Puede establecer la *entorno* en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="4575f-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="4575f-199">Incluyen valores definidos por el marco de trabajo `Development`, `Staging`, y `Production`.</span><span class="sxs-lookup"><span data-stu-id="4575f-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="4575f-200">Valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4575f-200">Values aren't case sensitive.</span></span> <span data-ttu-id="4575f-201">De forma predeterminada, el *entorno* se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="4575f-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="4575f-202">Cuando se usa [Visual Studio](https://www.visualstudio.com/), se pueden establecer variables de entorno el *launchSettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="4575f-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="4575f-203">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="4575f-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="4575f-206">Hospedaje de ensamblados de inicio</span><span class="sxs-lookup"><span data-stu-id="4575f-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="4575f-207">Establece los ensamblados de inicio de la aplicación host.</span><span class="sxs-lookup"><span data-stu-id="4575f-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="4575f-208">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="4575f-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="4575f-209">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-209">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-210">**Predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="4575f-210">**Default**: Empty string</span></span>  
<span data-ttu-id="4575f-211">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4575f-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="4575f-212">Una cadena delimitada por punto y coma de hospedaje de ensamblados de inicio para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="4575f-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="4575f-213">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4575f-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="4575f-214">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="4575f-215">Cuando se proporcionan hospedaje ensamblados de inicio, se agregan al ensamblado de la aplicación de carga cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="4575f-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-218">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4575f-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="4575f-219">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="4575f-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="4575f-220">Indica si el host debe escuchar en las direcciones URL configuradas con la `WebHostBuilder` en lugar de los que estén configurados con el `IServer` implementación.</span><span class="sxs-lookup"><span data-stu-id="4575f-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="4575f-221">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="4575f-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="4575f-222">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4575f-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4575f-223">**Predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="4575f-223">**Default**: true</span></span>  
<span data-ttu-id="4575f-224">**Establecer mediante**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="4575f-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="4575f-225">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4575f-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-228">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4575f-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="4575f-229">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="4575f-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="4575f-230">Impide que la carga automática de hospedaje de ensamblados de inicio, incluidos los ensamblados de inicio configurados por el ensamblado de la aplicación de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="4575f-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="4575f-231">Vea [agregar características de la aplicación desde un ensamblado externo con IHostingStartup](xref:hosting/ihostingstartup) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4575f-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="4575f-232">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="4575f-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="4575f-233">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="4575f-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="4575f-234">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="4575f-234">**Default**: false</span></span>  
<span data-ttu-id="4575f-235">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="4575f-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="4575f-236">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4575f-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-237">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-238">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-239">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4575f-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="4575f-240">Direcciones URL del servidor</span><span class="sxs-lookup"><span data-stu-id="4575f-240">Server URLs</span></span>

<span data-ttu-id="4575f-241">Indica las direcciones IP o direcciones de host con los puertos y protocolos que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="4575f-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="4575f-242">**Clave**: las direcciones URL</span><span class="sxs-lookup"><span data-stu-id="4575f-242">**Key**: urls</span></span>  
<span data-ttu-id="4575f-243">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-243">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-244">**Predeterminado**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="4575f-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="4575f-245">**Establecer mediante**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="4575f-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="4575f-246">Establézcalo separados por punto y coma (;) de prefijos de lista de direcciones URL que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="4575f-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="4575f-247">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="4575f-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="4575f-248">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto especificado y el protocolo (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="4575f-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="4575f-249">El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="4575f-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="4575f-250">Los formatos admitidos varían entre los servidores.</span><span class="sxs-lookup"><span data-stu-id="4575f-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="4575f-252">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="4575f-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="4575f-253">Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="4575f-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-254">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="4575f-255">Tiempo de espera de cierre</span><span class="sxs-lookup"><span data-stu-id="4575f-255">Shutdown Timeout</span></span>

<span data-ttu-id="4575f-256">Especifica la cantidad de tiempo de espera para el host de web en estado de cierre.</span><span class="sxs-lookup"><span data-stu-id="4575f-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="4575f-257">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="4575f-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="4575f-258">**Tipo de**: *int*</span><span class="sxs-lookup"><span data-stu-id="4575f-258">**Type**: *int*</span></span>  
<span data-ttu-id="4575f-259">**Predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="4575f-259">**Default**: 5</span></span>  
<span data-ttu-id="4575f-260">**Establecer mediante**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="4575f-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="4575f-261">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el `UseShutdownTimeout` método de extensión toma un `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="4575f-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="4575f-262">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="4575f-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-263">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-264">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-265">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="4575f-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="4575f-266">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="4575f-266">Startup Assembly</span></span>

<span data-ttu-id="4575f-267">Determina el ensamblado que se va a buscar la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="4575f-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="4575f-268">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="4575f-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="4575f-269">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-269">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-270">**Predeterminado**: ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4575f-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="4575f-271">**Establecer mediante**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="4575f-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="4575f-272">Puede hacer referencia al ensamblado por su nombre (`string`) o tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="4575f-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="4575f-273">Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="4575f-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-274">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-275">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="4575f-276">Raíz de Web</span><span class="sxs-lookup"><span data-stu-id="4575f-276">Web Root</span></span>

<span data-ttu-id="4575f-277">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="4575f-278">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="4575f-278">**Key**: webroot</span></span>  
<span data-ttu-id="4575f-279">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="4575f-279">**Type**: *string*</span></span>  
<span data-ttu-id="4575f-280">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Content Root)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="4575f-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="4575f-281">Si la ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva.</span><span class="sxs-lookup"><span data-stu-id="4575f-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="4575f-282">**Establecer mediante**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="4575f-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-284">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="4575f-285">Reemplazar configuración</span><span class="sxs-lookup"><span data-stu-id="4575f-285">Overriding configuration</span></span>

<span data-ttu-id="4575f-286">Use [configuración](xref:fundamentals/configuration/index) para configurar el host.</span><span class="sxs-lookup"><span data-stu-id="4575f-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="4575f-287">En el ejemplo siguiente, la configuración del host, opcionalmente, se especifica en un *hosting.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="4575f-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="4575f-288">Cualquier configuración que se carga desde el *hosting.json* archivo puede reemplazarse por los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="4575f-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="4575f-289">La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4575f-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4575f-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4575f-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4575f-292">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="4575f-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-293">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-294">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="4575f-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="4575f-295">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="4575f-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="4575f-296">El `UseConfiguration` no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="4575f-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="4575f-297">El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="4575f-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="4575f-298">El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="4575f-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="4575f-299">La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host.</span><span class="sxs-lookup"><span data-stu-id="4575f-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="4575f-300">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="4575f-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="4575f-301">Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="4575f-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="4575f-302">Para especificar el host que se ejecutan en una dirección URL determinada, podría pasar el valor deseado desde un símbolo del sistema al ejecutar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4575f-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="4575f-303">El argumento de línea de comandos reemplaza la `urls` valor de la *hosting.json* archivo y el servidor escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="4575f-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="4575f-304">Orden de importancia</span><span class="sxs-lookup"><span data-stu-id="4575f-304">Ordering importance</span></span>

<span data-ttu-id="4575f-305">Algunos de los `WebHostBuilder` configuración primero se lee desde variables de entorno, si establece.</span><span class="sxs-lookup"><span data-stu-id="4575f-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="4575f-306">Estas variables de entorno use el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="4575f-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="4575f-307">Para establecer las direcciones URL que el servidor escucha en de forma predeterminada, se establece `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="4575f-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="4575f-308">Puede invalidar cualquiera de estos valores de variable de entorno mediante la especificación de configuración (mediante `UseConfiguration`) o estableciendo el valor explícitamente (mediante `UseSetting` o uno de los métodos de extensión explícita, como `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="4575f-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="4575f-309">El host usa cualquier opción establece el valor de la última.</span><span class="sxs-lookup"><span data-stu-id="4575f-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="4575f-310">Si desea establecer la dirección URL predeterminada a un valor mediante programación pero permite que se reemplazará por la configuración, puede usar la configuración de línea de comandos después de establecer la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="4575f-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="4575f-311">Vea [configuración reemplazando](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="4575f-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="4575f-312">Iniciando el host</span><span class="sxs-lookup"><span data-stu-id="4575f-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4575f-313">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4575f-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4575f-314">**Run**</span><span class="sxs-lookup"><span data-stu-id="4575f-314">**Run**</span></span>

<span data-ttu-id="4575f-315">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:</span><span class="sxs-lookup"><span data-stu-id="4575f-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4575f-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="4575f-316">**Start**</span></span>

<span data-ttu-id="4575f-317">Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="4575f-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4575f-318">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="4575f-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="4575f-319">Puede inicializar e iniciar un nuevo host utilizando los valores predeterminados configurados previamente de `CreateDefaultBuilder` utilizando un método estáticos.</span><span class="sxs-lookup"><span data-stu-id="4575f-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="4575f-320">Estos métodos iniciar el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperar un salto (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="4575f-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="4575f-321">**Inicio (RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="4575f-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="4575f-322">Comience con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4575f-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4575f-323">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4575f-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4575f-324">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4575f-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4575f-325">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="4575f-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4575f-326">**Inicio (dirección url de cadena, RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="4575f-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="4575f-327">Comenzar con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4575f-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="4575f-328">Genera el mismo resultado que **inicio (aplicación RequestDelegate)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4575f-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="4575f-329">**Iniciar (acción<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4575f-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4575f-330">Usar una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="4575f-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="4575f-331">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4575f-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="4575f-332">Solicitud</span><span class="sxs-lookup"><span data-stu-id="4575f-332">Request</span></span>                                    | <span data-ttu-id="4575f-333">Respuesta</span><span class="sxs-lookup"><span data-stu-id="4575f-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="4575f-334">Hola, Martin</span><span class="sxs-lookup"><span data-stu-id="4575f-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="4575f-335">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="4575f-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="4575f-336">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="4575f-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="4575f-337">Produce una excepción con la cadena "EH oh!"</span><span class="sxs-lookup"><span data-stu-id="4575f-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="4575f-338">¡Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="4575f-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="4575f-339">Hello World!</span><span class="sxs-lookup"><span data-stu-id="4575f-339">Hello World!</span></span>                             |

<span data-ttu-id="4575f-340">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4575f-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4575f-341">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="4575f-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4575f-342">**Iniciar (dirección url, acción de cadena<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="4575f-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="4575f-343">Usar una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4575f-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="4575f-344">Genera el mismo resultado que **iniciar (acción<IRouteBuilder> routeBuilder)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4575f-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="4575f-345">**StartWith (acción<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="4575f-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4575f-346">Proporciona un delegado para configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4575f-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4575f-347">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4575f-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="4575f-348">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="4575f-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="4575f-349">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="4575f-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="4575f-350">**StartWith (dirección url, acción de cadena<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="4575f-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="4575f-351">Proporcione una dirección URL y un delegado que se va a configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4575f-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="4575f-352">Genera el mismo resultado que **StartWith (acción<IApplicationBuilder> aplicación)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="4575f-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4575f-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4575f-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4575f-354">**Run**</span><span class="sxs-lookup"><span data-stu-id="4575f-354">**Run**</span></span>

<span data-ttu-id="4575f-355">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:</span><span class="sxs-lookup"><span data-stu-id="4575f-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="4575f-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="4575f-356">**Start**</span></span>

<span data-ttu-id="4575f-357">Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="4575f-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="4575f-358">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="4575f-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="4575f-359">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="4575f-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="4575f-360">El [IHostingEnvironment interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de alojamiento web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4575f-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="4575f-361">Puede usar [inyección de constructor](xref:fundamentals/dependency-injection) para obtener el `IHostingEnvironment` para poder usar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="4575f-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="4575f-362">Puede usar un [enfoque basado en la convención](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="4575f-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="4575f-363">Como alternativa, también puede insertar el `IHostingEnvironment` en el `Startup` constructor para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4575f-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="4575f-364">Además el `IsDevelopment` método de extensión, `IHostingEnvironment` ofrece `IsStaging`, `IsProduction`, y `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="4575f-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="4575f-365">Vea [trabajar con varios entornos](xref:fundamentals/environments) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4575f-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="4575f-366">El `IHostingEnvironment` servicio también puede insertar directamente en el `Configure` método para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="4575f-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="4575f-367">También puede insertar `IHostingEnvironment` en el `Invoke` método al crear personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="4575f-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="4575f-368">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="4575f-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="4575f-369">El [IApplicationLifetime interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) le permite realizar las actividades posteriores a la de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="4575f-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="4575f-370">Tres propiedades en la interfaz son tokens de cancelación que puede registrar con `Action` métodos para definir los eventos de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="4575f-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="4575f-371">También hay un `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="4575f-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="4575f-372">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="4575f-372">Cancellation Token</span></span>    | <span data-ttu-id="4575f-373">Desencadene cuando &#8230;</span><span class="sxs-lookup"><span data-stu-id="4575f-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="4575f-374">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="4575f-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="4575f-375">El host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="4575f-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="4575f-376">Todavía pueden procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="4575f-376">Requests may still be processing.</span></span> <span data-ttu-id="4575f-377">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="4575f-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="4575f-378">El host está completando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="4575f-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="4575f-379">Todas las solicitudes se deben procesar completamente.</span><span class="sxs-lookup"><span data-stu-id="4575f-379">All requests should be completely processed.</span></span> <span data-ttu-id="4575f-380">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="4575f-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="4575f-381">Método</span><span class="sxs-lookup"><span data-stu-id="4575f-381">Method</span></span>            | <span data-ttu-id="4575f-382">Acción</span><span class="sxs-lookup"><span data-stu-id="4575f-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="4575f-383">Finalización de las solicitudes de la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="4575f-383">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="4575f-384">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="4575f-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="4575f-385">**Se aplica a los núcleos de ASP.NET 2.0 sólo**</span><span class="sxs-lookup"><span data-stu-id="4575f-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="4575f-386">Si compila el host insertando `IStartup` directamente en el contenedor de inyección de dependencia en lugar de llamar a `UseStartup` o `Configure`, puede encontrar el siguiente error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="4575f-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="4575f-387">Esto ocurre porque la [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="4575f-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="4575f-388">Si insertan manualmente `IStartup` en el contenedor de inyección de dependencia, agregue la llamada siguiente a su `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="4575f-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="4575f-389">O bien, agregar un ficticia `Configure` a su `WebHostBuilder`, que establece el `applicationName`(`ApplicationKey`) automáticamente:</span><span class="sxs-lookup"><span data-stu-id="4575f-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="4575f-390">**Tenga en cuenta**: Esto solo es necesario con la versión 2.0 de ASP.NET Core y solo cuando no llama al método `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="4575f-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="4575f-391">Para obtener más información, vea [anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) y [StartupInjection ejemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="4575f-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4575f-392">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4575f-392">Additional resources</span></span>

* [<span data-ttu-id="4575f-393">Publicar en Windows que usan IIS</span><span class="sxs-lookup"><span data-stu-id="4575f-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="4575f-394">Publicar en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="4575f-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="4575f-395">Publicar en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="4575f-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="4575f-396">Host en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="4575f-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
