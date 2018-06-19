---
title: Hospedaje en ASP.NET Core
author: guardrex
description: Obtenga información sobre el host de web en ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 3dab68da1b2b24351d40266429bf25f26ba467bf
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094806"
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="c48af-103">Hospedaje en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c48af-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="c48af-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c48af-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c48af-105">Las aplicaciones de ASP.NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="c48af-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="c48af-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c48af-107">Como mínimo, el host configura un servidor y una canalización de procesamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c48af-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="c48af-108">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="c48af-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="c48af-110">Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c48af-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="c48af-111">Normalmente, esto se realiza en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="c48af-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="c48af-112">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c48af-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="c48af-113">Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host:</span><span class="sxs-lookup"><span data-stu-id="c48af-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="c48af-114">`CreateDefaultBuilder` realiza las tareas siguientes:</span><span class="sxs-lookup"><span data-stu-id="c48af-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="c48af-115">Configura [Kestrel](servers/kestrel.md) como el servidor web.</span><span class="sxs-lookup"><span data-stu-id="c48af-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="c48af-116">Para conocer las opciones predeterminadas de Kestrel, consulte [la sección Opciones de Kestrel de la implementación de servidor web Kestrel en ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="c48af-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="c48af-117">Establece la raíz de contenido en la ruta de acceso devuelta por [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="c48af-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="c48af-118">Carga configuración opcional de:</span><span class="sxs-lookup"><span data-stu-id="c48af-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="c48af-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c48af-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="c48af-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="c48af-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="c48af-121">[Secretos del usuario](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="c48af-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="c48af-122">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="c48af-122">Environment variables.</span></span>
  * <span data-ttu-id="c48af-123">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c48af-123">Command-line arguments.</span></span>
* <span data-ttu-id="c48af-124">Configura el [registro](xref:fundamentals/logging/index) para la salida de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="c48af-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="c48af-125">El registro incluye reglas de [filtrado del registro](xref:fundamentals/logging/index#log-filtering) especificadas en una sección de configuración de registro de un archivo *appSettings.json* o *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="c48af-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="c48af-126">Cuando se ejecuta detrás de IIS, permite la [integración con IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="c48af-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="c48af-127">Configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c48af-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="c48af-128">El módulo crea a un proxy inverso entre IIS y Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c48af-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="c48af-129">También configura la aplicación para que [capture errores de inicio](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="c48af-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="c48af-130">Para conocer las opciones predeterminadas de IIS, consulte [la sección Opciones de IIS de Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="c48af-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="c48af-131">Establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c48af-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="c48af-132">Para más información, vea [Validación del ámbito](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="c48af-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="c48af-133">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="c48af-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c48af-134">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="c48af-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c48af-135">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c48af-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c48af-136">Para más información sobre la configuración de la aplicación, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c48af-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="c48af-137">Como alternativa al uso del método estático `CreateDefaultBuilder`, crear un host de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) es un enfoque compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="c48af-138">Para más información, vea la pestaña ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="c48af-140">Cree un host con una instancia de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c48af-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="c48af-141">La creación de un host se suele realizar en el punto de entrada de la aplicación, el método `Main`.</span><span class="sxs-lookup"><span data-stu-id="c48af-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="c48af-142">En las plantillas de proyecto, `Main` se encuentra en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c48af-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="c48af-143">`WebHostBuilder` requiere un [servidor que implemente IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c48af-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="c48af-144">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md) (antes del lanzamiento de ASP.NET Core 2.0, HTTP.sys se llamaba [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="c48af-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="c48af-145">En este ejemplo, el [método de extensión UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) especifica el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c48af-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="c48af-146">La *raíz del contenido* determina la ubicación en la que el host busca archivos de contenido como, por ejemplo, archivos de vista MVC.</span><span class="sxs-lookup"><span data-stu-id="c48af-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c48af-147">La raíz de contenido predeterminada se obtiene para `UseContentRoot` mediante [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="c48af-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="c48af-148">Cuando la aplicación se inicia desde la carpeta raíz del proyecto, esta se utiliza como la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="c48af-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c48af-149">Este es el valor predeterminado usado en [Visual Studio](https://www.visualstudio.com/) y las [nuevas plantillas dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c48af-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c48af-150">Para utilizar IIS como un proxy inverso, llame a [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="c48af-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="c48af-151">A diferencia de [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` no configura un *servidor*.</span><span class="sxs-lookup"><span data-stu-id="c48af-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="c48af-152">`UseIISIntegration` configura la ruta de acceso base y el puerto que escucha el servidor cuando se usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para crear un proxy inverso entre Kestrel e IIS.</span><span class="sxs-lookup"><span data-stu-id="c48af-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="c48af-153">Para utilizar IIS con ASP.NET Core, deben especificarse `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="c48af-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="c48af-154">`UseIISIntegration` solo se activa cuando se ejecuta en segundo plano de IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c48af-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="c48af-155">Para obtener más información, consulte [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="c48af-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="c48af-156">Una implementación mínima que configura un host (y una aplicación de ASP.NET Core) tendrá que especificar un servidor y la configuración de la canalización de solicitudes de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c48af-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="c48af-157">Al configurar un host, se pueden proporcionar los métodos [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) y [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="c48af-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="c48af-158">Si se especifica una clase `Startup`, debe definir un método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c48af-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="c48af-159">Para obtener más información, vea [Application Startup in ASP.NET Core](startup.md) (Inicio de la aplicación en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="c48af-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="c48af-160">Varias llamadas a `ConfigureServices` se anexan entre sí.</span><span class="sxs-lookup"><span data-stu-id="c48af-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="c48af-161">Varias llamadas a `Configure` o `UseStartup` en el `WebHostBuilder` reemplazan la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="c48af-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c48af-162">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="c48af-162">Host configuration values</span></span>

<span data-ttu-id="c48af-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="c48af-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="c48af-164">Configuración del generador de host, que incluye las variables de entorno con el formato `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="c48af-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="c48af-165">Por ejemplo: `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="c48af-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="c48af-166">Métodos explícitos, como `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="c48af-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="c48af-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="c48af-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="c48af-168">Al establecer un valor con `UseSetting`, el valor se establece como una cadena, independientemente del tipo.</span><span class="sxs-lookup"><span data-stu-id="c48af-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="c48af-169">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="c48af-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="c48af-170">Para obtener más información, consulte [Invalidación de la configuración](#overriding-configuration) en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c48af-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="c48af-171">Capturar errores de inicio</span><span class="sxs-lookup"><span data-stu-id="c48af-171">Capture Startup Errors</span></span>

<span data-ttu-id="c48af-172">Esta configuración controla la captura de errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="c48af-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="c48af-173">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="c48af-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="c48af-174">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c48af-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c48af-175">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="c48af-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="c48af-176">**Establecer mediante**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="c48af-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="c48af-177">**Variable de entorno**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="c48af-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="c48af-178">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="c48af-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="c48af-179">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="c48af-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="c48af-182">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="c48af-182">Content Root</span></span>

<span data-ttu-id="c48af-183">Esta configuración determina la ubicación en la que ASP.NET Core comienza a buscar archivos de contenido, como las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="c48af-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="c48af-184">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="c48af-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="c48af-185">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-185">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-186">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="c48af-187">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="c48af-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="c48af-188">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="c48af-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="c48af-189">La raíz de contenido también se usa como la ruta de acceso base para la [configuración de Raíz web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="c48af-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="c48af-190">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="c48af-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="c48af-193">Errores detallados</span><span class="sxs-lookup"><span data-stu-id="c48af-193">Detailed Errors</span></span>

<span data-ttu-id="c48af-194">Determina si se deben capturar los errores detallados.</span><span class="sxs-lookup"><span data-stu-id="c48af-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="c48af-195">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="c48af-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="c48af-196">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c48af-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c48af-197">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="c48af-197">**Default**: false</span></span>  
<span data-ttu-id="c48af-198">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c48af-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c48af-199">**Variable de entorno**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="c48af-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="c48af-200">Cuando se habilita (o cuando el <a href="#environment">entorno</a> está establecido en `Development`), la aplicación captura excepciones detalladas.</span><span class="sxs-lookup"><span data-stu-id="c48af-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="c48af-203">Entorno</span><span class="sxs-lookup"><span data-stu-id="c48af-203">Environment</span></span>

<span data-ttu-id="c48af-204">Establece el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-204">Sets the app's environment.</span></span>

<span data-ttu-id="c48af-205">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="c48af-205">**Key**: environment</span></span>  
<span data-ttu-id="c48af-206">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-206">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-207">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="c48af-207">**Default**: Production</span></span>  
<span data-ttu-id="c48af-208">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="c48af-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="c48af-209">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="c48af-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="c48af-210">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="c48af-210">The environment can be set to any value.</span></span> <span data-ttu-id="c48af-211">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="c48af-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="c48af-212">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c48af-212">Values aren't case sensitive.</span></span> <span data-ttu-id="c48af-213">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c48af-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="c48af-214">Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c48af-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="c48af-215">Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c48af-215">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="c48af-218">Ensamblados de inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="c48af-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="c48af-219">Establece los ensamblados de inicio de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="c48af-220">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="c48af-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="c48af-221">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-221">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-222">**Valor predeterminado**: una cadena vacía</span><span class="sxs-lookup"><span data-stu-id="c48af-222">**Default**: Empty string</span></span>  
<span data-ttu-id="c48af-223">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c48af-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c48af-224">**Variable de entorno**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c48af-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="c48af-225">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="c48af-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="c48af-226">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c48af-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="c48af-227">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="c48af-228">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="c48af-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-231">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="c48af-232">Preferir las direcciones URL de hospedaje</span><span class="sxs-lookup"><span data-stu-id="c48af-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="c48af-233">Indica si el host debe escuchar en las direcciones URL configuradas con `WebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="c48af-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="c48af-234">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="c48af-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="c48af-235">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c48af-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c48af-236">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="c48af-236">**Default**: true</span></span>  
<span data-ttu-id="c48af-237">**Establecer mediante**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="c48af-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="c48af-238">**Variable de entorno**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="c48af-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="c48af-239">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c48af-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-242">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="c48af-243">Evitar el inicio de hospedaje</span><span class="sxs-lookup"><span data-stu-id="c48af-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="c48af-244">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="c48af-245">Para más información, vea [Mejorar una aplicación desde un ensamblado externo](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="c48af-245">See [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="c48af-246">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="c48af-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="c48af-247">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="c48af-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c48af-248">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="c48af-248">**Default**: false</span></span>  
<span data-ttu-id="c48af-249">**Establecer mediante**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c48af-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c48af-250">**Variable de entorno**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="c48af-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="c48af-251">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c48af-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-254">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="c48af-255">Direcciones URL de servidor</span><span class="sxs-lookup"><span data-stu-id="c48af-255">Server URLs</span></span>

<span data-ttu-id="c48af-256">Indica las direcciones IP o las direcciones de host con los puertos y protocolos en que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c48af-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="c48af-257">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="c48af-257">**Key**: urls</span></span>  
<span data-ttu-id="c48af-258">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-258">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-259">**Predeterminado**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="c48af-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="c48af-260">**Establecer mediante**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="c48af-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="c48af-261">**Variable de entorno**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c48af-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="c48af-262">Se establece una lista de prefijos de URL separados por punto y coma (;) a los que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="c48af-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="c48af-263">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="c48af-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="c48af-264">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="c48af-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="c48af-265">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="c48af-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="c48af-266">Los formatos admitidos varían en función del servidor.</span><span class="sxs-lookup"><span data-stu-id="c48af-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="c48af-268">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="c48af-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="c48af-269">Para más información, vea [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration) (Implementación del servidor web de Kestrel en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="c48af-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="c48af-271">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="c48af-271">Shutdown Timeout</span></span>

<span data-ttu-id="c48af-272">Especifica la cantidad de tiempo que se espera hasta que se apague el host de web.</span><span class="sxs-lookup"><span data-stu-id="c48af-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="c48af-273">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="c48af-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="c48af-274">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="c48af-274">**Type**: *int*</span></span>  
<span data-ttu-id="c48af-275">**Valor predeterminado**: 5</span><span class="sxs-lookup"><span data-stu-id="c48af-275">**Default**: 5</span></span>  
<span data-ttu-id="c48af-276">**Establecer mediante**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="c48af-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="c48af-277">**Variable de entorno**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="c48af-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="c48af-278">Aunque la clave acepta un *int* con `UseSetting` (por ejemplo, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), el método de extensión [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) toma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="c48af-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="c48af-279">Esta característica es nueva en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c48af-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="c48af-280">Durante el período de tiempo de espera, el hospedaje:</span><span class="sxs-lookup"><span data-stu-id="c48af-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="c48af-281">Activa [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="c48af-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="c48af-282">Trata de detener los servicios hospedados y registra cualquier error que se produzca en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="c48af-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="c48af-283">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="c48af-284">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="c48af-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="c48af-285">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="c48af-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-288">Esta característica no está disponible en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="c48af-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="c48af-289">Ensamblado de inicio</span><span class="sxs-lookup"><span data-stu-id="c48af-289">Startup Assembly</span></span>

<span data-ttu-id="c48af-290">Determina el ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c48af-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="c48af-291">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="c48af-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="c48af-292">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-292">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-293">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c48af-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="c48af-294">**Establecer mediante**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="c48af-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="c48af-295">**Variable de entorno**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="c48af-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="c48af-296">Se puede hacer referencia al ensamblado por su nombre (`string`) o su tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="c48af-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="c48af-297">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="c48af-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="c48af-300">Raíz web</span><span class="sxs-lookup"><span data-stu-id="c48af-300">Web Root</span></span>

<span data-ttu-id="c48af-301">Establece la ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="c48af-302">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="c48af-302">**Key**: webroot</span></span>  
<span data-ttu-id="c48af-303">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="c48af-303">**Type**: *string*</span></span>  
<span data-ttu-id="c48af-304">**Valor predeterminado**: si no se especifica, el valor predeterminado es "(Raíz de contenido)/wwwroot", si existe la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="c48af-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="c48af-305">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="c48af-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="c48af-306">**Establecer mediante**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="c48af-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="c48af-307">**Variable de entorno**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="c48af-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="c48af-310">Invalidación de la configuración</span><span class="sxs-lookup"><span data-stu-id="c48af-310">Overriding configuration</span></span>

<span data-ttu-id="c48af-311">Use [Configuración](xref:fundamentals/configuration/index) para configurar el host.</span><span class="sxs-lookup"><span data-stu-id="c48af-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="c48af-312">En el ejemplo siguiente, la configuración del host se especifica de forma opcional en un archivo *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="c48af-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="c48af-313">Cualquier configuración que se carga desde el archivo *hosting.json* puede reemplazarse por argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c48af-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="c48af-314">La configuración generada (en `config`) se utiliza para configurar el host con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="c48af-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c48af-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="c48af-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="c48af-317">Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="c48af-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="c48af-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="c48af-320">Invalidar la configuración proporcionada por `UseUrls` primero con la configuración de *hosting.json* y segundo con la configuración del argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="c48af-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="c48af-321">El método de extensión [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="c48af-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="c48af-322">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="c48af-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="c48af-323">El método `UseConfiguration` espera que las claves coincidan con las claves `WebHostBuilder` (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="c48af-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="c48af-324">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="c48af-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="c48af-325">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="c48af-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="c48af-326">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="c48af-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="c48af-327">Para especificar el host que se ejecuta en una dirección URL determinada, se puede pasar el valor deseado desde un símbolo del sistema al ejecutar [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="c48af-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="c48af-328">El argumento de línea de comandos reemplaza el valor `urls` del archivo *hosting.json*, y el servidor escucha en el puerto 8080:</span><span class="sxs-lookup"><span data-stu-id="c48af-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="c48af-329">Inicio del host</span><span class="sxs-lookup"><span data-stu-id="c48af-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c48af-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c48af-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c48af-331">**Run**</span><span class="sxs-lookup"><span data-stu-id="c48af-331">**Run**</span></span>

<span data-ttu-id="c48af-332">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="c48af-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c48af-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="c48af-333">**Start**</span></span>

<span data-ttu-id="c48af-334">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="c48af-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c48af-335">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="c48af-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="c48af-336">La aplicación puede inicializar un nuevo host usando los valores preconfigurados de `CreateDefaultBuilder` mediante un método práctico estático.</span><span class="sxs-lookup"><span data-stu-id="c48af-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="c48af-337">Estos métodos inician el servidor sin la salida de la consola y con [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) esperando una interrupción (Ctrl-C/SIGINT o SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="c48af-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="c48af-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="c48af-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="c48af-339">Start con `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c48af-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c48af-340">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c48af-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c48af-341">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c48af-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c48af-342">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="c48af-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c48af-343">**Start(url de cadena, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="c48af-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="c48af-344">Start con una dirección URL y `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c48af-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c48af-345">Produce el mismo resultado que **Start(RequestDelegate app)**, excepto que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c48af-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="c48af-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c48af-346">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c48af-347">Use una instancia de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) para usar el middleware de enrutamiento:</span><span class="sxs-lookup"><span data-stu-id="c48af-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="c48af-348">Utilice las siguientes solicitudes de explorador con el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c48af-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="c48af-349">Solicitud</span><span class="sxs-lookup"><span data-stu-id="c48af-349">Request</span></span>                                    | <span data-ttu-id="c48af-350">Respuesta</span><span class="sxs-lookup"><span data-stu-id="c48af-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="c48af-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="c48af-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="c48af-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="c48af-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="c48af-353">Produce una excepción con la cadena "ooops!"</span><span class="sxs-lookup"><span data-stu-id="c48af-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="c48af-354">Produce una excepción con la cadena "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="c48af-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="c48af-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="c48af-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="c48af-356">Hello World!</span><span class="sxs-lookup"><span data-stu-id="c48af-356">Hello World!</span></span>                             |

<span data-ttu-id="c48af-357">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c48af-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c48af-358">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="c48af-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c48af-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c48af-359">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c48af-360">Use una dirección URL y una instancia de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c48af-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="c48af-361">Produce el mismo resultado que **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c48af-361">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="c48af-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="c48af-362">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c48af-363">Proporciona un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c48af-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c48af-364">Haga una solicitud en el explorador a `http://localhost:5000` para recibir la respuesta "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c48af-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c48af-365">`WaitForShutdown` se bloquea hasta que se emite un salto (Ctrl-C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="c48af-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c48af-366">La aplicación muestra el mensaje `Console.WriteLine` y espera a que se pulse una tecla para salir.</span><span class="sxs-lookup"><span data-stu-id="c48af-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c48af-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="c48af-367">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c48af-368">Proporcione una dirección URL y un delegado para configurar `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c48af-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c48af-369">Produce el mismo resultado que **StartWith(Action&lt;IApplicationBuilder&gt; app)**, salvo que la aplicación responde en `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c48af-369">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c48af-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c48af-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c48af-371">**Run**</span><span class="sxs-lookup"><span data-stu-id="c48af-371">**Run**</span></span>

<span data-ttu-id="c48af-372">El método `Run` inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que se apague el host:</span><span class="sxs-lookup"><span data-stu-id="c48af-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c48af-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="c48af-373">**Start**</span></span>

<span data-ttu-id="c48af-374">Ejecute el host de manera que se evite un bloqueo mediante una llamada a su método `Start`:</span><span class="sxs-lookup"><span data-stu-id="c48af-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c48af-375">Si se pasa una lista de direcciones URL al método `Start`, la escucha se produce en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="c48af-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="c48af-376">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="c48af-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="c48af-377">La [interfaz IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje web de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="c48af-378">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="c48af-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="c48af-379">Puede utilizarse un [enfoque convencional](xref:fundamentals/environments#startup-conventions) para configurar la aplicación en el inicio según el entorno.</span><span class="sxs-lookup"><span data-stu-id="c48af-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="c48af-380">Como alternativa, inserte `IHostingEnvironment` en el constructor `Startup` para su uso en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c48af-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="c48af-381">Además del método de extensión `IsDevelopment`, `IHostingEnvironment` ofrece los métodos `IsStaging`, `IsProduction` y `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="c48af-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="c48af-382">Para más información, vea [Usar varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c48af-382">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="c48af-383">El servicio `IHostingEnvironment` también se puede insertar directamente en el método `Configure` para configurar la canalización de procesamiento:</span><span class="sxs-lookup"><span data-stu-id="c48af-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="c48af-384">`IHostingEnvironment` puede insertarse en el método `Invoke` al crear [middleware](xref:fundamentals/middleware/index#writing-middleware) personalizado:</span><span class="sxs-lookup"><span data-stu-id="c48af-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="c48af-385">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="c48af-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="c48af-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permite actividades posteriores al inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="c48af-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="c48af-387">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="c48af-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="c48af-388">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="c48af-388">Cancellation Token</span></span>    | <span data-ttu-id="c48af-389">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="c48af-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="c48af-390">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="c48af-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="c48af-391">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="c48af-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="c48af-392">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c48af-392">Requests may still be processing.</span></span> <span data-ttu-id="c48af-393">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="c48af-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="c48af-394">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="c48af-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="c48af-395">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c48af-395">All requests should be processed.</span></span> <span data-ttu-id="c48af-396">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="c48af-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="c48af-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c48af-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="c48af-398">En la siguiente clase se usa `StopApplication` para cerrar correctamente una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="c48af-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="c48af-399">Validación del ámbito</span><span class="sxs-lookup"><span data-stu-id="c48af-399">Scope validation</span></span>

<span data-ttu-id="c48af-400">En ASP.NET Core 2.0 o posterior, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) establece [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) en `true` si el entorno de la aplicación es desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c48af-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="c48af-401">Cuando `ValidateScopes` está establecido en `true`, el proveedor de servicios predeterminado realiza comprobaciones para confirmar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c48af-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c48af-402">Los servicios con ámbito no se resuelven directa o indirectamente desde el proveedor de servicios raíz.</span><span class="sxs-lookup"><span data-stu-id="c48af-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c48af-403">Los servicios con ámbito no se insertan directa o indirectamente en singletons.</span><span class="sxs-lookup"><span data-stu-id="c48af-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c48af-404">El proveedor de servicios raíz se crea cuando se llama a [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="c48af-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="c48af-405">La vigencia del proveedor de servicios raíz es la misma que la de la aplicación o el servidor cuando el proveedor se inicia con la aplicación, y se elimina cuando la aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="c48af-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c48af-406">De la eliminación de los servicios con ámbito se encarga el contenedor que los creó.</span><span class="sxs-lookup"><span data-stu-id="c48af-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c48af-407">Si un servicio con ámbito se crea en el contenedor raíz, su vigencia sube a la del singleton, ya que solo lo puede eliminar el contenedor raíz cuando la aplicación o el servidor se cierran.</span><span class="sxs-lookup"><span data-stu-id="c48af-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c48af-408">Al validar los ámbitos de servicio, este tipo de situaciones se detectan cuando se llama a `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="c48af-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c48af-409">Para validar los ámbitos siempre, incluso en el entorno de producción, configure [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) con [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) en el generador de hosts:</span><span class="sxs-lookup"><span data-stu-id="c48af-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="c48af-410">Solución de problemas de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="c48af-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="c48af-411">**Se aplica únicamente a ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="c48af-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="c48af-412">Se puede crear un host mediante la inserción directa de `IStartup` en el contenedor de inserción de dependencias en lugar de llamar a `UseStartup` o `Configure`:</span><span class="sxs-lookup"><span data-stu-id="c48af-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="c48af-413">Si el host se crea de esta forma, puede producirse el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="c48af-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="c48af-414">Esto ocurre porque [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (el ensamblado actual) es necesario para buscar `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="c48af-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="c48af-415">Si la aplicación inserta manualmente `IStartup` en el contenedor de inserción de dependencias, agregue la siguiente llamada a `WebHostBuilder` con el nombre de ensamblado especificado:</span><span class="sxs-lookup"><span data-stu-id="c48af-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="c48af-416">O bien, agregue un `Configure` ficticio a `WebHostBuilder`, que establece `applicationName`(`ApplicationKey`) automáticamente:</span><span class="sxs-lookup"><span data-stu-id="c48af-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="c48af-417">**NOTA**: Esto solo es necesario con la versión ASP.NET Core 2.0 y únicamente cuando la aplicación no llama a `UseStartup` o `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c48af-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="c48af-418">Para obtener más información, vea [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Anuncios: Microsoft.Extensions.PlatformAbstractions ha sido eliminado (comentario) y el [ejemplo StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="c48af-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c48af-419">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c48af-419">Additional resources</span></span>

* [<span data-ttu-id="c48af-420">Hospedaje en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="c48af-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="c48af-421">Hospedaje en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="c48af-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="c48af-422">Hospedaje en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="c48af-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="c48af-423">Hospedaje en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="c48af-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
