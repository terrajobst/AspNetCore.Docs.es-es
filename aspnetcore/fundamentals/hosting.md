---
title: Hospedar en ASP.NET Core
author: guardrex
description: "Obtenga información acerca del host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación."
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="d044c-103">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d044c-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="d044c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d044c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d044c-105">Aplicaciones de ASP.NET Core configurar e iniciar un *host*.</span><span class="sxs-lookup"><span data-stu-id="d044c-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d044c-106">El host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d044c-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d044c-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="d044c-108">Configurar un host</span><span class="sxs-lookup"><span data-stu-id="d044c-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d044c-110">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d044c-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d044c-111">Normalmente, esto se realiza en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="d044c-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d044c-112">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d044c-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d044c-113">Una típica *Program.cs* llamadas [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="d044c-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="d044c-114">`CreateDefaultBuilder`realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d044c-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d044c-115">Configura [Kestrel](servers/kestrel.md) como el servidor web.</span><span class="sxs-lookup"><span data-stu-id="d044c-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="d044c-116">Para las opciones predeterminadas de Kestrel, consulte [el Kestrel opciones de sección de implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="d044c-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="d044c-117">Establece la raíz de contenido en la ruta de acceso devuelto por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d044c-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d044c-118">Configuración opcional de cargas de:</span><span class="sxs-lookup"><span data-stu-id="d044c-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="d044c-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d044c-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="d044c-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d044c-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d044c-121">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el `Development` entorno.</span><span class="sxs-lookup"><span data-stu-id="d044c-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="d044c-122">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="d044c-122">Environment variables.</span></span>
  * <span data-ttu-id="d044c-123">Argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d044c-123">Command-line arguments.</span></span>
* <span data-ttu-id="d044c-124">Configura [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="d044c-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d044c-125">El registro incluye [filtrado del registro](xref:fundamentals/logging/index#log-filtering) las reglas especificadas en una sección de configuración de registro de un *appSettings.JSON que se* o *appsettings. {} Entorno} .json* archivo.</span><span class="sxs-lookup"><span data-stu-id="d044c-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d044c-126">Cuando se ejecuta detrás de IIS, permite [integración con IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d044c-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d044c-127">Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d044c-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d044c-128">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d044c-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d044c-129">También configura la aplicación [capturar errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d044c-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d044c-130">Para las opciones predeterminadas IIS, consulte [IIS opciones de sección de Host de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="d044c-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="d044c-131">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="d044c-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d044c-132">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, la carpeta raíz del proyecto se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="d044c-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d044c-133">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d044c-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d044c-134">Para obtener más información sobre la configuración de la aplicación, consulte [configuración en ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d044c-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="d044c-135">Como alternativa al uso el método estático `CreateDefaultBuilder` (método), crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d044c-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d044c-136">Para obtener más información, consulte la ficha de 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d044c-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-138">Crear un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d044c-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d044c-139">Crear un host se suele realizar en el punto de entrada de la aplicación, el `Main` método.</span><span class="sxs-lookup"><span data-stu-id="d044c-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d044c-140">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d044c-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="d044c-141">`WebHostBuilder`requiere un [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d044c-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="d044c-142">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET 2.0 de núcleo, HTTP.sys llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d044c-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d044c-143">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d044c-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d044c-144">El *contenido raíz* determina que el host busca archivos de contenido, como los archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="d044c-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d044c-145">Se obtiene la raíz de contenido predeterminado para `UseContentRoot` por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d044c-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d044c-146">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, la carpeta raíz del proyecto se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="d044c-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d044c-147">Se trata de los valores predeterminados utilizados en [Visual Studio](https://www.visualstudio.com/) y [dotnet nuevas plantillas](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d044c-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d044c-148">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="d044c-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d044c-149">`UseIISIntegration`No configure un *server*, como [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="d044c-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d044c-150">`UseIISIntegration`configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="d044c-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d044c-151">Usar IIS con ASP.NET Core, `UseKestrel` y `UseIISIntegration` debe especificarse.</span><span class="sxs-lookup"><span data-stu-id="d044c-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d044c-152">`UseIISIntegration`solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d044c-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d044c-153">Para obtener más información, consulte [Introducción a ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module) y [referencia de configuración de ASP.NET Core módulo](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d044c-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="d044c-154">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de canalización de solicitud de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d044c-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d044c-155">Al configurar un host, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) se pueden proporcionar métodos.</span><span class="sxs-lookup"><span data-stu-id="d044c-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d044c-156">Si un `Startup` se especifica la clase, debe definir un `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="d044c-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d044c-157">Para obtener más información, consulte [inicio de la aplicación de ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="d044c-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="d044c-158">Varias llamadas a `ConfigureServices` anexar entre sí.</span><span class="sxs-lookup"><span data-stu-id="d044c-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d044c-159">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` sustituye la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="d044c-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d044c-160">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="d044c-160">Host configuration values</span></span>

<span data-ttu-id="d044c-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer valores de configuración de host:</span><span class="sxs-lookup"><span data-stu-id="d044c-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d044c-162">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d044c-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d044c-163">Por ejemplo: `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="d044c-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="d044c-164">Métodos explícitas, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="d044c-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="d044c-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="d044c-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d044c-166">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="d044c-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d044c-167">El host usa cualquier opción establece un valor de la última.</span><span class="sxs-lookup"><span data-stu-id="d044c-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d044c-168">Para obtener más información, consulte [configuración reemplazando](#overriding-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="d044c-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="d044c-169">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="d044c-169">Capture Startup Errors</span></span>

<span data-ttu-id="d044c-170">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="d044c-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d044c-171">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d044c-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d044c-172">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="d044c-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d044c-173">**Valor predeterminado**: valor predeterminado es `false` a menos que la aplicación se ejecuta con Kestrel detrás de IIS, donde el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="d044c-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d044c-174">**Establecer mediante**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d044c-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d044c-175">**Variable de entorno**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d044c-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d044c-176">Cuando `false`, errores durante el resultado de inicio en el host de salir.</span><span class="sxs-lookup"><span data-stu-id="d044c-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d044c-177">Cuando `true`, el host captura excepciones durante el inicio y se intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="d044c-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="d044c-180">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="d044c-180">Content Root</span></span>

<span data-ttu-id="d044c-181">Esta configuración determina donde ASP.NET Core comienza a buscar archivos de contenido, como las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="d044c-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d044c-182">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d044c-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="d044c-183">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-183">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-184">**Predeterminado**: valor predeterminado es la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d044c-185">**Establecer mediante**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d044c-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d044c-186">**Variable de entorno**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d044c-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d044c-187">La raíz de contenido también se usa como la ruta de acceso base para la [configuración Web raíz](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d044c-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d044c-188">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="d044c-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="d044c-191">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="d044c-191">Detailed Errors</span></span>

<span data-ttu-id="d044c-192">Determina si el registro detallado se deben capturar los errores.</span><span class="sxs-lookup"><span data-stu-id="d044c-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d044c-193">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d044c-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d044c-194">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="d044c-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d044c-195">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="d044c-195">**Default**: false</span></span>  
<span data-ttu-id="d044c-196">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d044c-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d044c-197">**Variable de entorno**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d044c-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d044c-198">Cuando habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="d044c-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="d044c-201">Entorno</span><span class="sxs-lookup"><span data-stu-id="d044c-201">Environment</span></span>

<span data-ttu-id="d044c-202">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-202">Sets the app's environment.</span></span>

<span data-ttu-id="d044c-203">**Clave**: entorno</span><span class="sxs-lookup"><span data-stu-id="d044c-203">**Key**: environment</span></span>  
<span data-ttu-id="d044c-204">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-204">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-205">**Predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="d044c-205">**Default**: Production</span></span>  
<span data-ttu-id="d044c-206">**Establecer mediante**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d044c-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d044c-207">**Variable de entorno**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d044c-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d044c-208">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="d044c-208">The environment can be set to any value.</span></span> <span data-ttu-id="d044c-209">Incluyen valores definidos por el marco de trabajo `Development`, `Staging`, y `Production`.</span><span class="sxs-lookup"><span data-stu-id="d044c-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d044c-210">Valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d044c-210">Values aren't case sensitive.</span></span> <span data-ttu-id="d044c-211">De forma predeterminada, el *entorno* se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="d044c-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d044c-212">Cuando se usa [Visual Studio](https://www.visualstudio.com/), se pueden establecer variables de entorno el *launchSettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="d044c-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d044c-213">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d044c-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d044c-216">Hospedaje de ensamblados de inicio</span><span class="sxs-lookup"><span data-stu-id="d044c-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d044c-217">Establece los ensamblados de inicio de la aplicación host.</span><span class="sxs-lookup"><span data-stu-id="d044c-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d044c-218">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d044c-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d044c-219">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-219">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-220">**Predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="d044c-220">**Default**: Empty string</span></span>  
<span data-ttu-id="d044c-221">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d044c-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d044c-222">**Variable de entorno**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d044c-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d044c-223">Una cadena delimitada por punto y coma de hospedaje de ensamblados de inicio para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="d044c-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="d044c-224">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d044c-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="d044c-225">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d044c-226">Cuando se especifican los ensamblados de inicio de hospedaje, estas se agregan al ensamblado de la aplicación de carga cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="d044c-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-229">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d044c-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d044c-230">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="d044c-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="d044c-231">Indica si el host debe escuchar en las direcciones URL configuradas con la `WebHostBuilder` en lugar de los que estén configurados con el `IServer` implementación.</span><span class="sxs-lookup"><span data-stu-id="d044c-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d044c-232">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d044c-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d044c-233">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="d044c-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d044c-234">**Predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="d044c-234">**Default**: true</span></span>  
<span data-ttu-id="d044c-235">**Establecer mediante**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d044c-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d044c-236">**Variable de entorno**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d044c-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="d044c-237">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d044c-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-240">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d044c-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d044c-241">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="d044c-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="d044c-242">Impide que la carga automática de hospedaje de ensamblados de inicio, incluidos los ensamblados de inicio configurados por el ensamblado de la aplicación de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="d044c-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d044c-243">Vea [agregar características de la aplicación desde un ensamblado externo con IHostingStartup](xref:host-and-deploy/ihostingstartup) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d044c-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="d044c-244">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d044c-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d044c-245">**Tipo de**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="d044c-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d044c-246">**Predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="d044c-246">**Default**: false</span></span>  
<span data-ttu-id="d044c-247">**Establecer mediante**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d044c-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d044c-248">**Variable de entorno**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d044c-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="d044c-249">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d044c-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-252">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d044c-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="d044c-253">Direcciones URL del servidor</span><span class="sxs-lookup"><span data-stu-id="d044c-253">Server URLs</span></span>

<span data-ttu-id="d044c-254">Indica las direcciones IP o direcciones de host con los puertos y protocolos que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d044c-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d044c-255">**Clave**: las direcciones URL</span><span class="sxs-lookup"><span data-stu-id="d044c-255">**Key**: urls</span></span>  
<span data-ttu-id="d044c-256">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-256">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-257">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d044c-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d044c-258">**Establecer mediante**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d044c-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d044c-259">**Variable de entorno**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d044c-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d044c-260">Establézcalo separados por punto y coma (;) de prefijos de lista de direcciones URL que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="d044c-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d044c-261">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d044c-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d044c-262">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto especificado y el protocolo (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d044c-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d044c-263">El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d044c-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d044c-264">Los formatos admitidos varían entre los servidores.</span><span class="sxs-lookup"><span data-stu-id="d044c-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="d044c-266">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="d044c-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d044c-267">Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="d044c-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="d044c-269">Tiempo de espera de cierre</span><span class="sxs-lookup"><span data-stu-id="d044c-269">Shutdown Timeout</span></span>

<span data-ttu-id="d044c-270">Especifica la cantidad de tiempo de espera para el host de web en estado de cierre.</span><span class="sxs-lookup"><span data-stu-id="d044c-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="d044c-271">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d044c-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d044c-272">**Tipo de**: *int*</span><span class="sxs-lookup"><span data-stu-id="d044c-272">**Type**: *int*</span></span>  
<span data-ttu-id="d044c-273">**Predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="d044c-273">**Default**: 5</span></span>  
<span data-ttu-id="d044c-274">**Establecer mediante**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d044c-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d044c-275">**Variable de entorno**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d044c-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d044c-276">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el `UseShutdownTimeout` método de extensión toma un `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d044c-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="d044c-277">Esta característica es nueva en ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="d044c-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-280">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d044c-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="d044c-281">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="d044c-281">Startup Assembly</span></span>

<span data-ttu-id="d044c-282">Determina el ensamblado que se va a buscar la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="d044c-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d044c-283">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d044c-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d044c-284">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-284">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-285">**Predeterminado**: ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="d044c-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d044c-286">**Establecer mediante**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d044c-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d044c-287">**Variable de entorno**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d044c-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d044c-288">El ensamblado por su nombre (`string`) o tipo (`TStartup`) puede hacer referencia a.</span><span class="sxs-lookup"><span data-stu-id="d044c-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d044c-289">Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="d044c-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="d044c-292">Raíz de Web</span><span class="sxs-lookup"><span data-stu-id="d044c-292">Web Root</span></span>

<span data-ttu-id="d044c-293">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d044c-294">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="d044c-294">**Key**: webroot</span></span>  
<span data-ttu-id="d044c-295">**Tipo de**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="d044c-295">**Type**: *string*</span></span>  
<span data-ttu-id="d044c-296">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Content Root)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="d044c-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d044c-297">Si la ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva.</span><span class="sxs-lookup"><span data-stu-id="d044c-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d044c-298">**Establecer mediante**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d044c-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d044c-299">**Variable de entorno**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d044c-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="d044c-302">Reemplazar configuración</span><span class="sxs-lookup"><span data-stu-id="d044c-302">Overriding configuration</span></span>

<span data-ttu-id="d044c-303">Use [configuración](xref:fundamentals/configuration/index) para configurar el host.</span><span class="sxs-lookup"><span data-stu-id="d044c-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="d044c-304">En el ejemplo siguiente, la configuración del host, opcionalmente, se especifica en un *hosting.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="d044c-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="d044c-305">Cualquier configuración que se carga desde el *hosting.json* archivo puede reemplazarse por los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d044c-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d044c-306">La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="d044c-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d044c-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="d044c-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d044c-309">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="d044c-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="d044c-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d044c-312">Reemplazar la configuración proporcionada por `UseUrls` con *hosting.json* configuración del primer argumento de línea de comandos de configuración segundo:</span><span class="sxs-lookup"><span data-stu-id="d044c-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="d044c-313">El [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d044c-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d044c-314">El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d044c-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d044c-315">El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d044c-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d044c-316">La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host.</span><span class="sxs-lookup"><span data-stu-id="d044c-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d044c-317">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="d044c-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d044c-318">Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d044c-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="d044c-319">Para especificar el host que se ejecutan en una dirección URL determinada, el valor deseado puede pasarse en desde un símbolo del sistema al ejecutar `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d044c-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="d044c-320">El argumento de línea de comandos reemplaza la `urls` valor de la *hosting.json* archivo y el servidor escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="d044c-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="d044c-321">Iniciando el host</span><span class="sxs-lookup"><span data-stu-id="d044c-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d044c-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d044c-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d044c-323">**Run**</span><span class="sxs-lookup"><span data-stu-id="d044c-323">**Run**</span></span>

<span data-ttu-id="d044c-324">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que cierre el host:</span><span class="sxs-lookup"><span data-stu-id="d044c-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d044c-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="d044c-325">**Start**</span></span>

<span data-ttu-id="d044c-326">Ejecute el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="d044c-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d044c-327">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="d044c-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d044c-328">La aplicación puede inicializar e iniciar un nuevo host utilizando los valores predeterminados configurados previamente de `CreateDefaultBuilder` utilizando un método estáticos.</span><span class="sxs-lookup"><span data-stu-id="d044c-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d044c-329">Estos métodos iniciar el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperar un salto (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d044c-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d044c-330">**Inicio (RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="d044c-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d044c-331">Comience con un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d044c-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d044c-332">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d044c-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d044c-333">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d044c-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d044c-334">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="d044c-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d044c-335">**Inicio (dirección url de cadena, RequestDelegate aplicación)**</span><span class="sxs-lookup"><span data-stu-id="d044c-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d044c-336">Comenzar con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d044c-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d044c-337">Genera el mismo resultado que **inicio (aplicación RequestDelegate)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d044c-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d044c-338">**Iniciar (acción<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d044c-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d044c-339">Usar una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="d044c-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d044c-340">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d044c-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d044c-341">Solicitud</span><span class="sxs-lookup"><span data-stu-id="d044c-341">Request</span></span>                                    | <span data-ttu-id="d044c-342">Respuesta</span><span class="sxs-lookup"><span data-stu-id="d044c-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d044c-343">Hola, Martin</span><span class="sxs-lookup"><span data-stu-id="d044c-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d044c-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d044c-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d044c-345">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d044c-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d044c-346">Produce una excepción con la cadena "EH oh!"</span><span class="sxs-lookup"><span data-stu-id="d044c-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d044c-347">¡Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d044c-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d044c-348">Hello World!</span><span class="sxs-lookup"><span data-stu-id="d044c-348">Hello World!</span></span>                             |

<span data-ttu-id="d044c-349">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d044c-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d044c-350">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="d044c-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d044c-351">**Iniciar (dirección url, acción de cadena<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d044c-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d044c-352">Usar una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d044c-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d044c-353">Genera el mismo resultado que **iniciar (acción<IRouteBuilder> routeBuilder)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d044c-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d044c-354">**StartWith (acción<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="d044c-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d044c-355">Proporciona un delegado para configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d044c-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d044c-356">Realizar una solicitud en el explorador para `http://localhost:5000` para recibir la respuesta "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d044c-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d044c-357">`WaitForShutdown`se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d044c-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d044c-358">La aplicación se muestra el `Console.WriteLine` mensaje y espera a que una pulsación de tecla salir.</span><span class="sxs-lookup"><span data-stu-id="d044c-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d044c-359">**StartWith (dirección url, acción de cadena<IApplicationBuilder> aplicación)**</span><span class="sxs-lookup"><span data-stu-id="d044c-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d044c-360">Proporcione una dirección URL y un delegado que se va a configurar un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d044c-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d044c-361">Genera el mismo resultado que **StartWith (acción<IApplicationBuilder> aplicación)**, excepto la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d044c-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d044c-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d044c-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d044c-363">**Run**</span><span class="sxs-lookup"><span data-stu-id="d044c-363">**Run**</span></span>

<span data-ttu-id="d044c-364">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="d044c-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d044c-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="d044c-365">**Start**</span></span>

<span data-ttu-id="d044c-366">Ejecute el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="d044c-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d044c-367">Si se pasa una lista de direcciones URL para el `Start` método, escucha en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="d044c-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d044c-368">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d044c-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="d044c-369">El [IHostingEnvironment interfaz](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de alojamiento web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d044c-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d044c-370">Usar [inyección de constructor](xref:fundamentals/dependency-injection) para obtener el `IHostingEnvironment` para poder usar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="d044c-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d044c-371">A [enfoque basado en la convención](xref:fundamentals/environments#startup-conventions) puede usarse para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="d044c-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d044c-372">Como alternativa, insertar el `IHostingEnvironment` en el `Startup` constructor para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d044c-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d044c-373">Además el `IsDevelopment` método de extensión, `IHostingEnvironment` ofrece `IsStaging`, `IsProduction`, y `IsEnvironment(string environmentName)` métodos.</span><span class="sxs-lookup"><span data-stu-id="d044c-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d044c-374">Vea [trabajar con varios entornos](xref:fundamentals/environments) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d044c-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="d044c-375">El `IHostingEnvironment` servicio también puede insertar directamente en el `Configure` método para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="d044c-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d044c-376">`IHostingEnvironment`puede insertar en el `Invoke` método al crear personalizado [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="d044c-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d044c-377">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d044c-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="d044c-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite para las actividades posteriores a la de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="d044c-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d044c-379">Tres propiedades en la interfaz son tokens de cancelación que se usa para registrar `Action` métodos que definen los eventos de inicio y cierre.</span><span class="sxs-lookup"><span data-stu-id="d044c-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="d044c-380">También hay un `StopApplication` método.</span><span class="sxs-lookup"><span data-stu-id="d044c-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="d044c-381">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="d044c-381">Cancellation Token</span></span>    | <span data-ttu-id="d044c-382">Desencadene cuando &#8230;</span><span class="sxs-lookup"><span data-stu-id="d044c-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d044c-383">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="d044c-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d044c-384">El host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="d044c-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d044c-385">Todavía pueden procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d044c-385">Requests may still be processing.</span></span> <span data-ttu-id="d044c-386">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="d044c-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d044c-387">El host está completando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="d044c-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d044c-388">Todas las solicitudes deben procesarse.</span><span class="sxs-lookup"><span data-stu-id="d044c-388">All requests should be processed.</span></span> <span data-ttu-id="d044c-389">Bloques de apagado hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="d044c-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="d044c-390">Método</span><span class="sxs-lookup"><span data-stu-id="d044c-390">Method</span></span>            | <span data-ttu-id="d044c-391">Acción</span><span class="sxs-lookup"><span data-stu-id="d044c-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="d044c-392">Finalización de las solicitudes de la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="d044c-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d044c-393">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d044c-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d044c-394">**Se aplica a los núcleos de ASP.NET 2.0 sólo**</span><span class="sxs-lookup"><span data-stu-id="d044c-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="d044c-395">Se puede crear un host insertando `IStartup` directamente en el contenedor de inyección de dependencia en lugar de llamar a `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d044c-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d044c-396">Si el host se crea de esta forma, puede producirse el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="d044c-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d044c-397">Esto ocurre porque la [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d044c-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d044c-398">Si la aplicación inserta manualmente `IStartup` en el contenedor de inyección de dependencia, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="d044c-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="d044c-399">O bien, agregar un ficticia `Configure` a la `WebHostBuilder`, que establece el `applicationName`(`ApplicationKey`) automáticamente:</span><span class="sxs-lookup"><span data-stu-id="d044c-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="d044c-400">**Tenga en cuenta**: Esto solo es necesario con la versión 2.0 de ASP.NET Core y solo cuando la aplicación no llama a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d044c-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="d044c-401">Para obtener más información, vea [anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) y [StartupInjection ejemplo](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d044c-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d044c-402">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d044c-402">Additional resources</span></span>

* [<span data-ttu-id="d044c-403">Hospedaje en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="d044c-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="d044c-404">Hospedaje en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="d044c-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="d044c-405">Hospedaje en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="d044c-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="d044c-406">Host en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="d044c-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
