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
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="581eb-104">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="581eb-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="581eb-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="581eb-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="581eb-106">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="581eb-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="581eb-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="581eb-108">Configurar un host</span><span class="sxs-lookup"><span data-stu-id="581eb-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="581eb-110">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="581eb-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="581eb-111">Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="581eb-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="581eb-112">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="581eb-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="581eb-113">Una típica *Program.cs* llamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="581eb-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="581eb-114">`CreateDefaultBuilder`realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="581eb-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="581eb-115">Configura [Kestrel](servers/kestrel.md) como el servidor web.</span><span class="sxs-lookup"><span data-stu-id="581eb-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="581eb-116">Para las opciones predeterminadas de Kestrel, consulte [el Kestrel opciones de sección de implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="581eb-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="581eb-117">Establece la raíz del contenido en [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="581eb-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="581eb-118">Configuración opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="581eb-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="581eb-119">*appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="581eb-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="581eb-120">*appSettings. {Entorno} .json*.</span><span class="sxs-lookup"><span data-stu-id="581eb-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="581eb-121">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el `Development` entorno.</span><span class="sxs-lookup"><span data-stu-id="581eb-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="581eb-122">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="581eb-122">Environment variables.</span></span>
  * <span data-ttu-id="581eb-123">Argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="581eb-123">Command-line arguments.</span></span>
* <span data-ttu-id="581eb-124">Configura [registro](xref:fundamentals/logging/index) de la salida de consola y de depuración a [filtrado del registro](xref:fundamentals/logging/index#log-filtering) las reglas especificadas en una sección de configuración de registro de un *appSettings.JSON que se* o *appsettings. {Entorno} .json* archivo.</span><span class="sxs-lookup"><span data-stu-id="581eb-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="581eb-125">Cuando se ejecuta detrás de IIS, permite [integración con IIS](xref:publishing/iis) mediante la configuración de la ruta de acceso base y el puerto del servidor debe escuchar al usar el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="581eb-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="581eb-126">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="581eb-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="581eb-127">También configura la aplicación [capturar errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="581eb-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="581eb-128">Para las opciones predeterminadas IIS, consulte [IIS opciones de sección de Host de ASP.NET Core en Windows con IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="581eb-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="581eb-129">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="581eb-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="581eb-130">La raíz de contenido de forma predeterminada es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="581eb-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="581eb-131">Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto).</span><span class="sxs-lookup"><span data-stu-id="581eb-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="581eb-132">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="581eb-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="581eb-133">Vea [configuración en ASP.NET Core](xref:fundamentals/configuration/index) para obtener más información sobre la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="581eb-134">Como alternativa al uso el método estático `CreateDefaultBuilder` (método), crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="581eb-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="581eb-135">Consulte la ficha de 1.x ASP.NET Core para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="581eb-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-137">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="581eb-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="581eb-138">Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="581eb-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="581eb-139">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="581eb-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="581eb-140">El siguiente *Program.cs* muestra cómo utilizar `WebHostBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="581eb-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="581eb-141">`WebHostBuilder`requiere un [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="581eb-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="581eb-142">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET 2.0 de núcleo, HTTP.sys llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="581eb-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="581eb-143">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="581eb-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="581eb-144">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="581eb-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="581eb-145">La raíz de contenido predeterminado proporcionado para `UseContentRoot` es [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="581eb-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="581eb-146">Esto resulta en el uso de carpeta raíz del proyecto web como la raíz de contenido cuando la aplicación se inicia desde la carpeta raíz (por ejemplo, al llamar a [dotnet ejecutar](/dotnet/core/tools/dotnet-run) desde la carpeta del proyecto).</span><span class="sxs-lookup"><span data-stu-id="581eb-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="581eb-147">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="581eb-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="581eb-148">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="581eb-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="581eb-149">`UseIISIntegration`No configure un *server*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="581eb-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="581eb-150">`UseIISIntegration`configura la ruta de acceso base y el puerto que debe escuchar el servidor cuando se usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="581eb-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="581eb-151">Para usar IIS con ASP.NET Core, debe especificar tanto `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="581eb-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="581eb-152">`UseIISIntegration`solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="581eb-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="581eb-153">Para obtener más información, consulte [Introducción a ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) y [referencia de configuración de ASP.NET Core módulo](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="581eb-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="581eb-154">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de canalización de solicitud de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="581eb-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="581eb-155">Al configurar un host, puede proporcionar [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) métodos.</span><span class="sxs-lookup"><span data-stu-id="581eb-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="581eb-156">Si especifica un `Startup` (clase), debe definir un `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="581eb-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="581eb-157">Para obtener más información, consulte [inicio de la aplicación de ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="581eb-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="581eb-158">Varias llamadas a `ConfigureServices` anexar entre sí.</span><span class="sxs-lookup"><span data-stu-id="581eb-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="581eb-159">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` sustituye la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="581eb-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="581eb-160">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="581eb-160">Host configuration values</span></span>

<span data-ttu-id="581eb-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) proporciona métodos para establecer la mayoría de los valores de configuración disponibles para el host, que también se puede establecer directamente con [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="581eb-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="581eb-162">Al establecer un valor con `UseSetting`, el valor se establece como una cadena (entre comillas), independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="581eb-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="581eb-163">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="581eb-163">Capture Startup Errors</span></span>

<span data-ttu-id="581eb-164">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="581eb-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="581eb-165">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="581eb-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="581eb-166">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="581eb-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="581eb-167">**Valor predeterminado**: valor predeterminado es `false` a menos que la aplicación se ejecuta con Kestrel detrás de IIS, donde el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="581eb-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="581eb-168">**Establecer mediante**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="581eb-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="581eb-169">Cuando `false`, errores durante el resultado de inicio en el host de salir.</span><span class="sxs-lookup"><span data-stu-id="581eb-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="581eb-170">Cuando `true`, el host captura excepciones durante el inicio y se intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="581eb-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="581eb-173">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="581eb-173">Content Root</span></span>

<span data-ttu-id="581eb-174">Esta configuración determina donde ASP.NET Core comienza a buscar archivos de contenido, como las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="581eb-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="581eb-175">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="581eb-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="581eb-176">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-176">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-177">**Predeterminado**: valor predeterminado es la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="581eb-178">**Establecer mediante**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="581eb-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="581eb-179">La raíz de contenido también se usa como la ruta de acceso base para la [configuración Web raíz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="581eb-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="581eb-180">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="581eb-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="581eb-183">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="581eb-183">Detailed Errors</span></span>

<span data-ttu-id="581eb-184">Determina si el registro detallado se deben capturar los errores.</span><span class="sxs-lookup"><span data-stu-id="581eb-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="581eb-185">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="581eb-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="581eb-186">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="581eb-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="581eb-187">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="581eb-187">**Default**: false</span></span>  
<span data-ttu-id="581eb-188">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="581eb-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="581eb-189">Cuando habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="581eb-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="581eb-192">Entorno</span><span class="sxs-lookup"><span data-stu-id="581eb-192">Environment</span></span>

<span data-ttu-id="581eb-193">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-193">Sets the app's environment.</span></span>

<span data-ttu-id="581eb-194">**Clave**: entorno</span><span class="sxs-lookup"><span data-stu-id="581eb-194">**Key**: environment</span></span>  
<span data-ttu-id="581eb-195">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-195">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-196">**Predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="581eb-196">**Default**: Production</span></span>  
<span data-ttu-id="581eb-197">**Establecer mediante**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="581eb-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="581eb-198">Puede establecer la *entorno* en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="581eb-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="581eb-199">Incluyen valores definidos por el marco de trabajo `Development`, `Staging`, y `Production`.</span><span class="sxs-lookup"><span data-stu-id="581eb-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="581eb-200">Valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="581eb-200">Values aren't case sensitive.</span></span> <span data-ttu-id="581eb-201">De forma predeterminada, el *entorno* se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="581eb-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="581eb-202">Cuando se usa [Visual Studio](https://www.visualstudio.com/), se pueden establecer variables de entorno el *launchSettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="581eb-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="581eb-203">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="581eb-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="581eb-206">Hospedaje de ensamblados de inicio</span><span class="sxs-lookup"><span data-stu-id="581eb-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="581eb-207">Establece los ensamblados de inicio de la aplicación host.</span><span class="sxs-lookup"><span data-stu-id="581eb-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="581eb-208">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="581eb-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="581eb-209">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-209">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-210">**Predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="581eb-210">**Default**: Empty string</span></span>  
<span data-ttu-id="581eb-211">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="581eb-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="581eb-212">Una cadena delimitada por punto y coma de hospedaje de ensamblados de inicio para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="581eb-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="581eb-213">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="581eb-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="581eb-214">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="581eb-215">Cuando se proporcionan hospedaje ensamblados de inicio, se agregan al ensamblado de la aplicación de carga cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="581eb-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-218">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="581eb-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="581eb-219">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="581eb-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="581eb-220">Indica si el host debe escuchar en las direcciones URL configuradas con la `WebHostBuilder` en lugar de los que estén configurados con el `IServer` implementación.</span><span class="sxs-lookup"><span data-stu-id="581eb-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="581eb-221">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="581eb-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="581eb-222">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="581eb-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="581eb-223">**Predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="581eb-223">**Default**: true</span></span>  
<span data-ttu-id="581eb-224">**Establecer mediante**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="581eb-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="581eb-225">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="581eb-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-228">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="581eb-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="581eb-229">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="581eb-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="581eb-230">Impide que la carga automática de ensamblados de inicio, incluidos los ensamblados de la aplicación de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="581eb-230">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="581eb-231">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="581eb-231">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="581eb-232">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="581eb-232">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="581eb-233">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="581eb-233">**Default**: false</span></span>  
<span data-ttu-id="581eb-234">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="581eb-234">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="581eb-235">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="581eb-235">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-236">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-236">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-237">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-237">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-238">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="581eb-238">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="581eb-239">Direcciones URL del servidor</span><span class="sxs-lookup"><span data-stu-id="581eb-239">Server URLs</span></span>

<span data-ttu-id="581eb-240">Indica las direcciones IP o direcciones de host con los puertos y protocolos que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="581eb-240">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="581eb-241">**Clave**: las direcciones URL</span><span class="sxs-lookup"><span data-stu-id="581eb-241">**Key**: urls</span></span>  
<span data-ttu-id="581eb-242">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-242">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-243">**Predeterminado**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="581eb-243">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="581eb-244">**Establecer mediante**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="581eb-244">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="581eb-245">Establézcalo separados por punto y coma (;) de prefijos de lista de direcciones URL que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="581eb-245">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="581eb-246">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="581eb-246">For example, `http://localhost:123`.</span></span> <span data-ttu-id="581eb-247">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto especificado y el protocolo (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="581eb-247">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="581eb-248">El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="581eb-248">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="581eb-249">Los formatos admitidos varían entre los servidores.</span><span class="sxs-lookup"><span data-stu-id="581eb-249">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="581eb-251">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="581eb-251">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="581eb-252">Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="581eb-252">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="581eb-254">Tiempo de espera de cierre</span><span class="sxs-lookup"><span data-stu-id="581eb-254">Shutdown Timeout</span></span>

<span data-ttu-id="581eb-255">Especifica la cantidad de tiempo de espera para el host de web en estado de cierre.</span><span class="sxs-lookup"><span data-stu-id="581eb-255">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="581eb-256">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="581eb-256">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="581eb-257">**Tipo de**: *int*</span><span class="sxs-lookup"><span data-stu-id="581eb-257">**Type**: *int*</span></span>  
<span data-ttu-id="581eb-258">**Predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="581eb-258">**Default**: 5</span></span>  
<span data-ttu-id="581eb-259">**Establecer mediante**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="581eb-259">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="581eb-260">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el `UseShutdownTimeout` método de extensión toma un `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="581eb-260">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="581eb-261">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="581eb-261">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-262">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-262">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-263">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-263">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-264">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="581eb-264">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="581eb-265">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="581eb-265">Startup Assembly</span></span>

<span data-ttu-id="581eb-266">Determina el ensamblado que se va a buscar la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="581eb-266">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="581eb-267">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="581eb-267">**Key**: startupAssembly</span></span>  
<span data-ttu-id="581eb-268">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-268">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-269">**Predeterminado**: ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="581eb-269">**Default**: The app's assembly</span></span>  
<span data-ttu-id="581eb-270">**Establecer mediante**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="581eb-270">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="581eb-271">Puede hacer referencia al ensamblado por su nombre (`string`) o tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="581eb-271">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="581eb-272">Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="581eb-272">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-273">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-274">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-274">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="581eb-275">Raíz de Web</span><span class="sxs-lookup"><span data-stu-id="581eb-275">Web Root</span></span>

<span data-ttu-id="581eb-276">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-276">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="581eb-277">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="581eb-277">**Key**: webroot</span></span>  
<span data-ttu-id="581eb-278">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="581eb-278">**Type**: *string*</span></span>  
<span data-ttu-id="581eb-279">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Content Root)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="581eb-279">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="581eb-280">Si la ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva.</span><span class="sxs-lookup"><span data-stu-id="581eb-280">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="581eb-281">**Establecer mediante**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="581eb-281">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-282">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-282">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-283">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-283">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="581eb-284">Reemplazar configuración</span><span class="sxs-lookup"><span data-stu-id="581eb-284">Overriding configuration</span></span>

<span data-ttu-id="581eb-285">Use [configuración](xref:fundamentals/configuration/index) para configurar el host.</span><span class="sxs-lookup"><span data-stu-id="581eb-285">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="581eb-286">En el ejemplo siguiente, la configuración del host, opcionalmente, se especifica en un *hosting.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="581eb-286">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="581eb-287">Cualquier configuración que se carga desde el *hosting.json* archivo puede reemplazarse por los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="581eb-287">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="581eb-288">La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="581eb-288">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-289">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-289">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="581eb-290">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="581eb-290">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="581eb-291">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="581eb-291">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-293">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="581eb-293">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="581eb-294">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="581eb-294">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="581eb-295">El `UseConfiguration` no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="581eb-295">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="581eb-296">El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="581eb-296">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="581eb-297">El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="581eb-297">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="581eb-298">La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host.</span><span class="sxs-lookup"><span data-stu-id="581eb-298">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="581eb-299">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="581eb-299">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="581eb-300">Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="581eb-300">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="581eb-301">Para especificar el host que se ejecutan en una dirección URL determinada, podría pasar el valor deseado desde un símbolo del sistema al ejecutar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="581eb-301">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="581eb-302">El argumento de línea de comandos reemplaza la `urls` valor de la *hosting.json* archivo y el servidor escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="581eb-302">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="581eb-303">Orden de importancia</span><span class="sxs-lookup"><span data-stu-id="581eb-303">Ordering importance</span></span>

<span data-ttu-id="581eb-304">Algunos de los `WebHostBuilder` configuración primero se lee desde variables de entorno, si establece.</span><span class="sxs-lookup"><span data-stu-id="581eb-304">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="581eb-305">Estas variables de entorno use el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="581eb-305">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="581eb-306">Para establecer las direcciones URL que el servidor escucha en de forma predeterminada, se establece `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="581eb-306">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="581eb-307">Puede invalidar cualquiera de estos valores de variable de entorno mediante la especificación de configuración (mediante `UseConfiguration`) o estableciendo el valor explícitamente (mediante `UseSetting` o uno de los métodos de extensión explícita, como `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="581eb-307">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="581eb-308">El host usa cualquier opción establece el valor de la última.</span><span class="sxs-lookup"><span data-stu-id="581eb-308">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="581eb-309">Si desea establecer la dirección URL predeterminada a un valor mediante programación pero permite que se reemplazará por la configuración, puede usar la configuración de línea de comandos después de establecer la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="581eb-309">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="581eb-310">Vea [configuración reemplazando](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="581eb-310">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="581eb-311">Iniciando el host</span><span class="sxs-lookup"><span data-stu-id="581eb-311">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="581eb-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="581eb-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="581eb-313">**Run**</span><span class="sxs-lookup"><span data-stu-id="581eb-313">**Run**</span></span>

<span data-ttu-id="581eb-314">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:</span><span class="sxs-lookup"><span data-stu-id="581eb-314">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="581eb-315">**Start**</span><span class="sxs-lookup"><span data-stu-id="581eb-315">**Start**</span></span>

<span data-ttu-id="581eb-316">Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="581eb-316">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="581eb-317">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="581eb-317">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="581eb-318">Puede inicializar e iniciar un nuevo host utilizando los valores predeterminados configurados previamente de `CreateDefaultBuilder` utilizando un método estáticos.</span><span class="sxs-lookup"><span data-stu-id="581eb-318">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="581eb-319">Estos métodos iniciar el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperar un salto (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="581eb-319">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="581eb-320">**Inicio (RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="581eb-320">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="581eb-321">Comience con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="581eb-321">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="581eb-322">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="581eb-322">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="581eb-323">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="581eb-323">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="581eb-324">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="581eb-324">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="581eb-325">**Inicio (dirección url de cadena, RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="581eb-325">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="581eb-326">Comenzar con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="581eb-326">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="581eb-327">Genera el mismo resultado que **inicio (aplicación RequestDelegate)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="581eb-327">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="581eb-328">**Iniciar (acción<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="581eb-328">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="581eb-329">Usar una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="581eb-329">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="581eb-330">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="581eb-330">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="581eb-331">Solicitud</span><span class="sxs-lookup"><span data-stu-id="581eb-331">Request</span></span>                                    | <span data-ttu-id="581eb-332">Respuesta</span><span class="sxs-lookup"><span data-stu-id="581eb-332">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="581eb-333">Hola, Martin</span><span class="sxs-lookup"><span data-stu-id="581eb-333">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="581eb-334">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="581eb-334">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="581eb-335">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="581eb-335">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="581eb-336">Produce una excepción con la cadena "EH oh!"</span><span class="sxs-lookup"><span data-stu-id="581eb-336">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="581eb-337">¡Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="581eb-337">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="581eb-338">Hello World!</span><span class="sxs-lookup"><span data-stu-id="581eb-338">Hello World!</span></span>                             |

<span data-ttu-id="581eb-339">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="581eb-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="581eb-340">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="581eb-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="581eb-341">**Iniciar (dirección url, acción de cadena<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="581eb-341">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="581eb-342">Usar una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="581eb-342">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="581eb-343">Genera el mismo resultado que **iniciar (acción<IRouteBuilder> routeBuilder)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="581eb-343">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="581eb-344">**StartWith (acción<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="581eb-344">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="581eb-345">Proporciona un delegado para configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="581eb-345">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="581eb-346">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="581eb-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="581eb-347">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="581eb-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="581eb-348">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="581eb-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="581eb-349">**StartWith (dirección url, acción de cadena<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="581eb-349">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="581eb-350">Proporcione una dirección URL y un delegado que se va a configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="581eb-350">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="581eb-351">Genera el mismo resultado que **StartWith (acción<IApplicationBuilder> aplicación)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="581eb-351">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="581eb-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="581eb-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="581eb-353">**Run**</span><span class="sxs-lookup"><span data-stu-id="581eb-353">**Run**</span></span>

<span data-ttu-id="581eb-354">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:</span><span class="sxs-lookup"><span data-stu-id="581eb-354">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="581eb-355">**Start**</span><span class="sxs-lookup"><span data-stu-id="581eb-355">**Start**</span></span>

<span data-ttu-id="581eb-356">Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="581eb-356">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="581eb-357">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="581eb-357">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="581eb-358">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="581eb-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="581eb-359">El [IHostingEnvironment interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de alojamiento web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="581eb-359">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="581eb-360">Puede usar [inyección de constructor](xref:fundamentals/dependency-injection) para obtener el `IHostingEnvironment` para poder usar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="581eb-360">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="581eb-361">Puede usar un [enfoque basado en la convención](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="581eb-361">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="581eb-362">Como alternativa, también puede insertar el `IHostingEnvironment` en el `Startup` constructor para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="581eb-362">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="581eb-363">Además el `IsDevelopment` método de extensión, `IHostingEnvironment` ofrece `IsStaging`, `IsProduction`, y `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="581eb-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="581eb-364">Vea [trabajar con varios entornos](xref:fundamentals/environments) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="581eb-364">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="581eb-365">El `IHostingEnvironment` servicio también puede insertar directamente en el `Configure` método para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="581eb-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="581eb-366">También puede insertar `IHostingEnvironment` en el `Invoke` método al crear personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="581eb-366">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="581eb-367">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="581eb-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="581eb-368">El [IApplicationLifetime interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) le permite realizar las actividades posteriores a la de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="581eb-368">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="581eb-369">Tres propiedades en la interfaz son tokens de cancelación que puede registrar con `Action` métodos para definir los eventos de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="581eb-369">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="581eb-370">También hay un `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="581eb-370">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="581eb-371">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="581eb-371">Cancellation Token</span></span>    | <span data-ttu-id="581eb-372">Desencadene cuando &#8230;</span><span class="sxs-lookup"><span data-stu-id="581eb-372">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="581eb-373">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="581eb-373">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="581eb-374">El host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="581eb-374">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="581eb-375">Todavía pueden procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="581eb-375">Requests may still be processing.</span></span> <span data-ttu-id="581eb-376">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="581eb-376">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="581eb-377">El host está completando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="581eb-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="581eb-378">Todas las solicitudes se deben procesar completamente.</span><span class="sxs-lookup"><span data-stu-id="581eb-378">All requests should be completely processed.</span></span> <span data-ttu-id="581eb-379">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="581eb-379">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="581eb-380">Método</span><span class="sxs-lookup"><span data-stu-id="581eb-380">Method</span></span>            | <span data-ttu-id="581eb-381">Acción</span><span class="sxs-lookup"><span data-stu-id="581eb-381">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="581eb-382">Finalización de las solicitudes de la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="581eb-382">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="581eb-383">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="581eb-383">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="581eb-384">**Se aplica a los núcleos de ASP.NET 2.0 sólo**</span><span class="sxs-lookup"><span data-stu-id="581eb-384">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="581eb-385">Si compila el host insertando `IStartup` directamente en el contenedor de inyección de dependencia en lugar de llamar a `UseStartup` o `Configure`, puede encontrar el siguiente error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="581eb-385">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="581eb-386">Esto ocurre porque la [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="581eb-386">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="581eb-387">Si insertan manualmente `IStartup` en el contenedor de inyección de dependencia, agregue la llamada siguiente a su `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="581eb-387">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="581eb-388">O bien, agregar un ficticia `Configure` a su `WebHostBuilder`, que establece el `applicationName`(`ApplicationKey`) automáticamente:</span><span class="sxs-lookup"><span data-stu-id="581eb-388">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="581eb-389">**Tenga en cuenta**: Esto solo es necesario con la versión 2.0 de ASP.NET Core y solo cuando no llama al método `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="581eb-389">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="581eb-390">Para obtener más información, vea [anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) y [StartupInjection ejemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="581eb-390">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="581eb-391">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="581eb-391">Additional resources</span></span>

* [<span data-ttu-id="581eb-392">Publicar en Windows que usan IIS</span><span class="sxs-lookup"><span data-stu-id="581eb-392">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="581eb-393">Publicar en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="581eb-393">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="581eb-394">Publicar en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="581eb-394">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="581eb-395">Host en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="581eb-395">Host in a Windows Service</span></span>](xref:hosting/windows-service)
