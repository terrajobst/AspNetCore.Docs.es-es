---
title: Host genérico de .NET
author: rick-anderson
description: Obtenga información sobre el host genérico .NET Core, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: f14917ad924e2c762a14c2cb5f51391d4be06e7b
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378751"
---
# <a name="net-generic-host"></a><span data-ttu-id="9ba3e-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="9ba3e-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9ba3e-104">En este artículo se presenta el host genérico .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) y se proporciona orientación sobre cómo usarlo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="9ba3e-105">¿Qué es un host?</span><span class="sxs-lookup"><span data-stu-id="9ba3e-105">What's a host?</span></span>

<span data-ttu-id="9ba3e-106">El *host* es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="9ba3e-107">Inserción de dependencias (ID)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="9ba3e-108">Registro</span><span class="sxs-lookup"><span data-stu-id="9ba3e-108">Logging</span></span>
* <span data-ttu-id="9ba3e-109">Configuración</span><span class="sxs-lookup"><span data-stu-id="9ba3e-109">Configuration</span></span>
* <span data-ttu-id="9ba3e-110">Implementaciones de `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-110">`IHostedService` implementations</span></span>

<span data-ttu-id="9ba3e-111">Cuando se inicia un host, llama a `IHostedService.StartAsync` en cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> que encuentra en el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="9ba3e-112">En una aplicación web, una de las implementaciones de `IHostedService` es un servicio web que inicia una [implementación de servidor HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="9ba3e-113">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="9ba3e-114">En las versiones de ASP.NET Core anteriores a la 3.0, el [host web](xref:fundamentals/host/web-host) se usa para cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="9ba3e-115">El host web ya no se recomienda para las aplicaciones web, pero sigue estando disponible para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9ba3e-116">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-116">Set up a host</span></span>

<span data-ttu-id="9ba3e-117">Normalmente se configura, compila y ejecuta el host por el código de la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="9ba3e-118">El método `Main` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-118">The `Main` method:</span></span>

* <span data-ttu-id="9ba3e-119">Llama a un método `CreateHostBuilder` para crear y configurar un objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="9ba3e-120">Llama a los métodos `Build` y `Run` en el objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="9ba3e-121">Este es el código de *Program.cs* para una carga de trabajo no HTTP, con una sola implementación de `IHostedService` agregada para el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="9ba3e-122">Para una carga de trabajo HTTP, el método `Main` es el mismo, pero `CreateHostBuilder` llama a `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="9ba3e-123">Si la aplicación usa Entity Framework Core, no cambie el nombre o la firma del método `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="9ba3e-124">Las [herramientas de Entity Framework Core](/ef/core/miscellaneous/cli/) esperan encontrar un método `CreateHostBuilder` que configure el host sin ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="9ba3e-125">Para obtener más información, consulte [Creación de DbContext en tiempo de diseño](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="9ba3e-126">Configuración predeterminada del generador</span><span class="sxs-lookup"><span data-stu-id="9ba3e-126">Default builder settings</span></span>

<span data-ttu-id="9ba3e-127">El método <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="9ba3e-128">Establece la [raíz de contenido](xref:fundamentals/index#content-root) en la ruta de acceso devuelta por <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="9ba3e-129">Carga la configuración de host de:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="9ba3e-130">Variables de entorno con el prefijo "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="9ba3e-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="9ba3e-131">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-131">Command-line arguments.</span></span>
* <span data-ttu-id="9ba3e-132">Carga la configuración de aplicación de:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="9ba3e-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="9ba3e-134">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="9ba3e-135">[Administrador de secretos](xref:security/app-secrets), cuando la aplicación se ejecuta en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="9ba3e-136">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-136">Environment variables.</span></span>
  * <span data-ttu-id="9ba3e-137">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-137">Command-line arguments.</span></span>
* <span data-ttu-id="9ba3e-138">Agrega los siguientes proveedores de [registro](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="9ba3e-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="9ba3e-139">Consola</span><span class="sxs-lookup"><span data-stu-id="9ba3e-139">Console</span></span>
  * <span data-ttu-id="9ba3e-140">Depuración</span><span class="sxs-lookup"><span data-stu-id="9ba3e-140">Debug</span></span>
  * <span data-ttu-id="9ba3e-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="9ba3e-141">EventSource</span></span>
  * <span data-ttu-id="9ba3e-142">EventLog (solo si se ejecuta en Windows)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="9ba3e-143">Permite la [validación del ámbito](xref:fundamentals/dependency-injection#scope-validation) y la [validación de dependencias](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) si el entorno es Desarrollo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="9ba3e-144">El método `ConfigureWebHostDefaults` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="9ba3e-145">Carga la configuración de host de las variables de entorno con el prefijo "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="9ba3e-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="9ba3e-146">Establece el servidor [Kestrel](xref:fundamentals/servers/kestrel) como servidor web y lo configura por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="9ba3e-147">Para conocer las opciones predeterminadas del servidor Kestrel, consulte <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="9ba3e-148">Agrega el [middleware de filtrado de hosts](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="9ba3e-149">Agrega el [middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) si ASPNETCORE_FORWARDEDHEADERS_ENABLED es "true".</span><span class="sxs-lookup"><span data-stu-id="9ba3e-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="9ba3e-150">Permite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-150">Enables IIS integration.</span></span> <span data-ttu-id="9ba3e-151">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="9ba3e-152">En las secciones [Configuración para todos los tipos de aplicaciones](#settings-for-all-app-types) y [Configuración para las aplicaciones web](#settings-for-web-apps), más adelante en este artículo, se muestra cómo invalidar la configuración predeterminada del generador.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="9ba3e-153">Servicios proporcionados por el marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="9ba3e-153">Framework-provided services</span></span>

<span data-ttu-id="9ba3e-154">Los servicios que se registran automáticamente incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="9ba3e-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="9ba3e-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="9ba3e-157">IHostEnvironment/IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="9ba3e-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="9ba3e-158">Para más información sobre los servicios proporcionados por el marco, consulte <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="9ba3e-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="9ba3e-160">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (anteriormente, `IApplicationLifetime`) en cualquier clase para controlar las tareas posteriores al inicio y el cierre estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="9ba3e-161">Tres de las propiedades de la interfaz son tokens de cancelación que se usan para registrar los métodos del controlador de eventos de inicio y detención de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="9ba3e-162">La interfaz también incluye un método `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="9ba3e-163">El ejemplo siguiente es una implementación de `IHostedService` que registra los eventos `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="9ba3e-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-164">IHostLifetime</span></span>

<span data-ttu-id="9ba3e-165">La implementación de <xref:Microsoft.Extensions.Hosting.IHostLifetime> controla cuándo se inicia el host y cuándo se detiene.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="9ba3e-166">Se usa la última implementación registrada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-166">The last implementation registered is used.</span></span>

<span data-ttu-id="9ba3e-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` es la implementación predeterminada de `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="9ba3e-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="9ba3e-169">escucha Ctrl+C/SIGINT o SIGTERM y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="9ba3e-170">Desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="9ba3e-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="9ba3e-171">IHostEnvironment</span></span>

<span data-ttu-id="9ba3e-172">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostEnvironment> en una clase para obtener información sobre lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="9ba3e-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="9ba3e-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="9ba3e-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="9ba3e-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="9ba3e-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="9ba3e-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="9ba3e-176">Las aplicaciones web implementan la interfaz de `IWebHostEnvironment`, que hereda `IHostEnvironment` y agrega:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="9ba3e-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="9ba3e-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="9ba3e-178">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-178">Host configuration</span></span>

<span data-ttu-id="9ba3e-179">La configuración de host se usa para las propiedades de la implementación de <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="9ba3e-180">La configuración de host está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration), en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="9ba3e-181">Después de `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` se reemplaza por la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="9ba3e-182">Para agregar la configuración de host, llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="9ba3e-183">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9ba3e-184">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="9ba3e-185">El proveedor de variables de entorno con el prefijo `DOTNET_` y los argumentos de línea de comandos se incluyen mediante CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="9ba3e-186">Para las aplicaciones web, se agrega el proveedor de variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="9ba3e-187">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="9ba3e-188">Por ejemplo, el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="9ba3e-189">En el ejemplo siguiente se crea la configuración de host:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="9ba3e-190">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="9ba3e-190">App configuration</span></span>

<span data-ttu-id="9ba3e-191">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="9ba3e-192">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="9ba3e-193">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="9ba3e-194">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y como servicio de ID.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="9ba3e-195">La configuración de host también se agrega a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="9ba3e-196">Para más información, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="9ba3e-197">Configuración para todos los tipos de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="9ba3e-197">Settings for all app types</span></span>

<span data-ttu-id="9ba3e-198">En esta sección se enumeran las configuraciones de host que se aplican a las cargas de trabajo HTTP y no HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="9ba3e-199">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="9ba3e-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="9ba3e-200">ApplicationName</span></span>

<span data-ttu-id="9ba3e-201">La propiedad [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) se establece desde la configuración de host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="9ba3e-202">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="9ba3e-202">**Key**: applicationName</span></span>  
<span data-ttu-id="9ba3e-203">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-203">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-204">**Predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="9ba3e-205">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="9ba3e-206">Para establecer este valor, use la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="9ba3e-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="9ba3e-207">ContentRootPath</span></span>

<span data-ttu-id="9ba3e-208">La propiedad [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="9ba3e-209">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="9ba3e-210">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9ba3e-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="9ba3e-211">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-211">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-212">**Predeterminado**: carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="9ba3e-213">**Variable de entorno**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="9ba3e-214">Para establecer este valor, use la variable de entorno o llame a `UseContentRoot` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="9ba3e-215">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-215">For more information, see:</span></span>

* [<span data-ttu-id="9ba3e-216">Aspectos básicos: Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="9ba3e-216">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="9ba3e-217">WebRoot</span><span class="sxs-lookup"><span data-stu-id="9ba3e-217">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="9ba3e-218">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="9ba3e-218">EnvironmentName</span></span>

<span data-ttu-id="9ba3e-219">La propiedad [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) puede establecerse en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-219">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="9ba3e-220">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-220">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9ba3e-221">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-221">Values aren't case sensitive.</span></span>

<span data-ttu-id="9ba3e-222">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="9ba3e-222">**Key**: environment</span></span>  
<span data-ttu-id="9ba3e-223">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-223">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-224">**Valor predeterminado**: Producción</span><span class="sxs-lookup"><span data-stu-id="9ba3e-224">**Default**: Production</span></span>  
<span data-ttu-id="9ba3e-225">**Variable de entorno**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-225">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="9ba3e-226">Para establecer este valor, use la variable de entorno o llame a `UseEnvironment` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-226">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="9ba3e-227">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="9ba3e-227">ShutdownTimeout</span></span>

<span data-ttu-id="9ba3e-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="9ba3e-229">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-229">The default value is five seconds.</span></span>  <span data-ttu-id="9ba3e-230">Durante el período de tiempo de espera, el host:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-230">During the timeout period, the host:</span></span>

* <span data-ttu-id="9ba3e-231">Activa [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-231">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="9ba3e-232">Trata de detener los servicios hospedados y registra los errores que se producen en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-232">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="9ba3e-233">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-233">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="9ba3e-234">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-234">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="9ba3e-235">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-235">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="9ba3e-236">**Clave**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="9ba3e-236">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="9ba3e-237">**Tipo**: *int*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-237">**Type**: *int*</span></span>  
<span data-ttu-id="9ba3e-238">**Predeterminado**: 5 segundos **Variable de entorno**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-238">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="9ba3e-239">Para establecer este valor, use la variable de entorno o configure `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="9ba3e-240">El siguiente ejemplo establece el tiempo de espera en 20 segundos:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="9ba3e-241">Configuración para las aplicaciones web</span><span class="sxs-lookup"><span data-stu-id="9ba3e-241">Settings for web apps</span></span>

<span data-ttu-id="9ba3e-242">Algunas configuraciones de host solo se aplican a las cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-242">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="9ba3e-243">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-243">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="9ba3e-244">Los métodos de extensión en `IWebHostBuilder` están disponibles para estas configuraciones.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-244">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="9ba3e-245">Los ejemplos de código que muestran cómo llamar a los métodos de extensión suponen que `webBuilder` es una instancia de `IWebHostBuilder`, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-245">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="9ba3e-246">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="9ba3e-246">CaptureStartupErrors</span></span>

<span data-ttu-id="9ba3e-247">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-247">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9ba3e-248">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-248">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="9ba3e-249">**Clave**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="9ba3e-249">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="9ba3e-250">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-250">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9ba3e-251">**Valor predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-251">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="9ba3e-252">**Variable de entorno**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-252">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="9ba3e-253">Para establecer este valor, utilice la configuración o llame a `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-253">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="9ba3e-254">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="9ba3e-254">DetailedErrors</span></span>

<span data-ttu-id="9ba3e-255">Si se habilita, o si el entorno es `Development`, la aplicación captura errores detallados.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-255">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="9ba3e-256">**Clave**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="9ba3e-256">**Key**: detailedErrors</span></span>  
<span data-ttu-id="9ba3e-257">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-257">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9ba3e-258">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="9ba3e-258">**Default**: false</span></span>  
<span data-ttu-id="9ba3e-259">**Variable de entorno**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-259">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="9ba3e-260">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-260">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="9ba3e-261">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="9ba3e-261">HostingStartupAssemblies</span></span>

<span data-ttu-id="9ba3e-262">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-262">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="9ba3e-263">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-263">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="9ba3e-264">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-264">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="9ba3e-265">**Clave**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="9ba3e-265">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="9ba3e-266">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-266">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-267">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="9ba3e-267">**Default**: Empty string</span></span>  
<span data-ttu-id="9ba3e-268">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-268">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="9ba3e-269">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-269">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="9ba3e-270">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="9ba3e-270">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="9ba3e-271">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-271">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="9ba3e-272">**Clave**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="9ba3e-272">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="9ba3e-273">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-273">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-274">**Valor predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="9ba3e-274">**Default**: Empty string</span></span>  
<span data-ttu-id="9ba3e-275">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-275">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="9ba3e-276">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-276">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="9ba3e-277">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="9ba3e-277">HTTPS_Port</span></span>

<span data-ttu-id="9ba3e-278">Puerto de redireccionamiento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-278">The HTTPS redirect port.</span></span> <span data-ttu-id="9ba3e-279">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-279">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="9ba3e-280">**Clave**: https_port</span><span class="sxs-lookup"><span data-stu-id="9ba3e-280">**Key**: https_port</span></span>  
<span data-ttu-id="9ba3e-281">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-281">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-282">**Predeterminado**: no se ha establecido ningún valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-282">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="9ba3e-283">**Variable de entorno**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-283">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="9ba3e-284">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-284">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="9ba3e-285">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="9ba3e-285">PreferHostingUrls</span></span>

<span data-ttu-id="9ba3e-286">Indica si el host debe escuchar en las direcciones URL configuradas con `IWebHostBuilder` en lugar de las que estén configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-286">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="9ba3e-287">**Clave**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="9ba3e-287">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="9ba3e-288">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-288">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9ba3e-289">**Valor predeterminado**: true</span><span class="sxs-lookup"><span data-stu-id="9ba3e-289">**Default**: true</span></span>  
<span data-ttu-id="9ba3e-290">**Variable de entorno**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-290">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="9ba3e-291">Para establecer este valor, use la variable de entorno o llame a `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-291">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="9ba3e-292">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="9ba3e-292">PreventHostingStartup</span></span>

<span data-ttu-id="9ba3e-293">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-293">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="9ba3e-294">Para más información, consulte <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-294">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="9ba3e-295">**Clave**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="9ba3e-295">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="9ba3e-296">**Tipo**: *bool* (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-296">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9ba3e-297">**Valor predeterminado**: false</span><span class="sxs-lookup"><span data-stu-id="9ba3e-297">**Default**: false</span></span>  
<span data-ttu-id="9ba3e-298">**Variable de entorno**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-298">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="9ba3e-299">Para establecer este valor, use la variable de entorno o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-299">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="9ba3e-300">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="9ba3e-300">StartupAssembly</span></span>

<span data-ttu-id="9ba3e-301">Ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-301">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="9ba3e-302">**Clave**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="9ba3e-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="9ba3e-303">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-303">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-304">**Valor predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="9ba3e-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="9ba3e-305">**Variable de entorno**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-305">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="9ba3e-306">Para establecer este valor, use la variable de entorno o llame a `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-306">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="9ba3e-307">`UseStartup` puede tomar un nombre del ensamblado (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-307">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="9ba3e-308">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="9ba3e-309">Direcciones URL</span><span class="sxs-lookup"><span data-stu-id="9ba3e-309">URLs</span></span>

<span data-ttu-id="9ba3e-310">Lista delimitada por punto y coma de las direcciones IP o las direcciones de host con los puertos y protocolos en los que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-310">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="9ba3e-311">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-311">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9ba3e-312">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-312">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="9ba3e-313">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-313">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9ba3e-314">Los formatos admitidos varían de un servidor a otro.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-314">Supported formats vary among servers.</span></span>

<span data-ttu-id="9ba3e-315">**Clave**: urls</span><span class="sxs-lookup"><span data-stu-id="9ba3e-315">**Key**: urls</span></span>  
<span data-ttu-id="9ba3e-316">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-316">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-317">**Predeterminado**: `http://localhost:5000` y `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-317">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="9ba3e-318">**Variable de entorno**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-318">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="9ba3e-319">Para establecer este valor, use la variable de entorno o llame a `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-319">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="9ba3e-320">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-320">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="9ba3e-321">Para más información, consulte <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-321">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="9ba3e-322">WebRoot</span><span class="sxs-lookup"><span data-stu-id="9ba3e-322">WebRoot</span></span>

<span data-ttu-id="9ba3e-323">Ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-323">The relative path to the app's static assets.</span></span>

<span data-ttu-id="9ba3e-324">**Clave**: webroot</span><span class="sxs-lookup"><span data-stu-id="9ba3e-324">**Key**: webroot</span></span>  
<span data-ttu-id="9ba3e-325">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-325">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-326">**Predeterminado**: De manera predeterminada, es `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-326">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="9ba3e-327">Debe existir la ruta de acceso a *{raíz del contenido}/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-327">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="9ba3e-328">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-328">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="9ba3e-329">**Variable de entorno**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-329">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="9ba3e-330">Para establecer este valor, use la variable de entorno o llame a `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-330">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="9ba3e-331">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-331">For more information, see:</span></span>

* [<span data-ttu-id="9ba3e-332">Aspectos básicos: Raíz web</span><span class="sxs-lookup"><span data-stu-id="9ba3e-332">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="9ba3e-333">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="9ba3e-333">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="9ba3e-334">Administración de la vigencia del host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-334">Manage the host lifetime</span></span>

<span data-ttu-id="9ba3e-335">Llame a los métodos en la implementación de <xref:Microsoft.Extensions.Hosting.IHost> creada para iniciar y detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-335">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="9ba3e-336">Estos métodos afectan a todas las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-336">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="9ba3e-337">Run</span><span class="sxs-lookup"><span data-stu-id="9ba3e-337">Run</span></span>

<span data-ttu-id="9ba3e-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="9ba3e-339">RunAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-339">RunAsync</span></span>

<span data-ttu-id="9ba3e-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="9ba3e-341">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-341">RunConsoleAsync</span></span>

<span data-ttu-id="9ba3e-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre Ctrl+C/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="9ba3e-343">Iniciar</span><span class="sxs-lookup"><span data-stu-id="9ba3e-343">Start</span></span>

<span data-ttu-id="9ba3e-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="9ba3e-345">StartAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-345">StartAsync</span></span>

<span data-ttu-id="9ba3e-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia el host y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="9ba3e-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de `StartAsync`, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="9ba3e-348">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-348">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="9ba3e-349">StopAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-349">StopAsync</span></span>

<span data-ttu-id="9ba3e-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="9ba3e-351">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="9ba3e-351">WaitForShutdown</span></span>

<span data-ttu-id="9ba3e-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> bloquea el subproceso de llamada hasta que IHostLifetime desencadena el cierre, por ejemplo, a través de Ctrl+C/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="9ba3e-353">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-353">WaitForShutdownAsync</span></span>

<span data-ttu-id="9ba3e-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="9ba3e-355">Control externo</span><span class="sxs-lookup"><span data-stu-id="9ba3e-355">External control</span></span>

<span data-ttu-id="9ba3e-356">El control directo de la vigencia del host se puede lograr mediante métodos a los que se puede llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-356">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9ba3e-357">Las aplicaciones ASP.NET Core configuran e inician un host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-357">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="9ba3e-358">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-358">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="9ba3e-359">En este artículo se trata el host genérico de ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), que se usa para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-359">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="9ba3e-360">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una mayor variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-360">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="9ba3e-361">La mensajería, las tareas en segundo plano y otras cargas de trabajo que no son de HTTP basadas en la ventaja de host genérico se benefician de funcionalidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-361">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="9ba3e-362">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-362">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="9ba3e-363">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-363">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="9ba3e-364">El host genérico reemplazará al host web en una próxima versión y actuará como la API de host principal tanto en escenarios de HTTP como los que no lo son.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-364">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="9ba3e-365">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9ba3e-365">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9ba3e-366">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-366">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="9ba3e-367">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-367">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="9ba3e-368">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-368">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="9ba3e-369">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-369">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="9ba3e-370">En la configuración de **.NET Core Launch (console)** , busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-370">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="9ba3e-371">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-371">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="9ba3e-372">Introducción</span><span class="sxs-lookup"><span data-stu-id="9ba3e-372">Introduction</span></span>

<span data-ttu-id="9ba3e-373">La biblioteca de host genérico está disponible en el espacio de nombres <xref:Microsoft.Extensions.Hosting> y la proporciona el paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-373">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="9ba3e-374">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-374">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="9ba3e-375"><xref:Microsoft.Extensions.Hosting.IHostedService> es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-375"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="9ba3e-376">Cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-376">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="9ba3e-377">Se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> en cada <xref:Microsoft.Extensions.Hosting.IHostedService> cuando se inicia el host, y se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9ba3e-378">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-378">Set up a host</span></span>

<span data-ttu-id="9ba3e-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="9ba3e-380">Opciones</span><span class="sxs-lookup"><span data-stu-id="9ba3e-380">Options</span></span>

<span data-ttu-id="9ba3e-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configura opciones para <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="9ba3e-382">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="9ba3e-382">Shutdown timeout</span></span>

<span data-ttu-id="9ba3e-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="9ba3e-384">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-384">The default value is five seconds.</span></span>

<span data-ttu-id="9ba3e-385">La siguiente configuración de opción en `Program.Main` aumenta el tiempo de espera de apagado predeterminado de 5 segundos a 20 segundos:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-385">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="9ba3e-386">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="9ba3e-386">Default services</span></span>

<span data-ttu-id="9ba3e-387">Estos son los servicios que se registran durante la inicialización del host:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-387">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="9ba3e-388">[Entorno](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-388">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="9ba3e-389">[Configuración](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-389">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="9ba3e-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="9ba3e-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="9ba3e-392">[Opciones](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-392">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="9ba3e-393">[Registro](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-393">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="9ba3e-394">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-394">Host configuration</span></span>

<span data-ttu-id="9ba3e-395">La configuración del host se crea:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-395">Host configuration is created by:</span></span>

* <span data-ttu-id="9ba3e-396">Llamando a métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder> para establecer la [raíz del contenido](#content-root) y el [entorno](#environment).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-396">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="9ba3e-397">Leyendo la configuración desde los proveedores de configuración de <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-397">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="9ba3e-398">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="9ba3e-398">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="9ba3e-399">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="9ba3e-399">Application key (name)</span></span>

<span data-ttu-id="9ba3e-400">La propiedad [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) se establece desde la configuración del host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-400">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="9ba3e-401">Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="9ba3e-401">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="9ba3e-402">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="9ba3e-402">**Key**: applicationName</span></span>  
<span data-ttu-id="9ba3e-403">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-403">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-404">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-404">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="9ba3e-405">**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-405">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="9ba3e-406">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9ba3e-406">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="9ba3e-407">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="9ba3e-407">Content root</span></span>

<span data-ttu-id="9ba3e-408">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-408">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="9ba3e-409">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9ba3e-409">**Key**: contentRoot</span></span>  
<span data-ttu-id="9ba3e-410">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-410">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-411">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-411">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9ba3e-412">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-412">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9ba3e-413">**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9ba3e-413">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="9ba3e-414">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-414">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="9ba3e-415">Para más información, consulte [Aspectos básicos: Raíz del contenido](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-415">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="9ba3e-416">Entorno</span><span class="sxs-lookup"><span data-stu-id="9ba3e-416">Environment</span></span>

<span data-ttu-id="9ba3e-417">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-417">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9ba3e-418">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="9ba3e-418">**Key**: environment</span></span>  
<span data-ttu-id="9ba3e-419">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="9ba3e-419">**Type**: *string*</span></span>  
<span data-ttu-id="9ba3e-420">**Valor predeterminado**: Producción</span><span class="sxs-lookup"><span data-stu-id="9ba3e-420">**Default**: Production</span></span>  
<span data-ttu-id="9ba3e-421">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9ba3e-421">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9ba3e-422">**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="9ba3e-422">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="9ba3e-423">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-423">The environment can be set to any value.</span></span> <span data-ttu-id="9ba3e-424">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-424">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9ba3e-425">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-425">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="9ba3e-426">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="9ba3e-426">ConfigureHostConfiguration</span></span>

<span data-ttu-id="9ba3e-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para el host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="9ba3e-428">La configuración del host se usa para inicializar el <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-428">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="9ba3e-429">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="9ba3e-430">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-430">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="9ba3e-431">De forma predeterminada no se incluye ningún proveedor.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-431">No providers are included by default.</span></span> <span data-ttu-id="9ba3e-432">Debe especificar de forma explícita los proveedores de configuración que requiere la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, así como:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-432">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="9ba3e-433">La configuración de archivo (por ejemplo, de un archivo *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-433">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="9ba3e-434">La configuración de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-434">Environment variable configuration.</span></span>
* <span data-ttu-id="9ba3e-435">La configuración del argumento de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-435">Command-line argument configuration.</span></span>
* <span data-ttu-id="9ba3e-436">Otros proveedores de configuración necesarios.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-436">Any other required configuration providers.</span></span>

<span data-ttu-id="9ba3e-437">La configuración de archivo del host se habilita especificando la ruta base de la aplicación con `SetBasePath` seguido de una llamada a uno de los [proveedores de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-437">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="9ba3e-438">La aplicación de ejemplo usa un archivo JSON, *hostsettings.json*, y llama a <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> para consumir los valores de configuración del host del archivo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-438">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="9ba3e-439">Para agregar una [configuración de la variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) del host, llame a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el generador del host.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-439">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="9ba3e-440">`AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-440">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="9ba3e-441">La aplicación de ejemplo usa el prefijo `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-441">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="9ba3e-442">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-442">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="9ba3e-443">Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-443">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="9ba3e-444">Durante el desarrollo, al usar [Visual Studio](https://visualstudio.microsoft.com) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-444">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="9ba3e-445">En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-445">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="9ba3e-446">Para más información, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-446">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="9ba3e-447">La [configuración de la línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) se agrega llamando a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-447">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="9ba3e-448">La configuración de la línea de comandos se agrega en último lugar para permitir que los argumentos de la línea de comandos invaliden la configuración proporcionada por los proveedores de configuración anteriores.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-448">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="9ba3e-449">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-449">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="9ba3e-450">Se puede proporcionar una configuración adicional con las claves [applicationName](#application-key-name) y [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-450">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="9ba3e-451">Configuración de `HostBuilder` de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-451">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="9ba3e-452">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="9ba3e-452">ConfigureAppConfiguration</span></span>

<span data-ttu-id="9ba3e-453">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en la implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="9ba3e-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="9ba3e-455">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="9ba3e-456">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-456">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="9ba3e-457">La configuración creada por <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y en <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-457">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="9ba3e-458">La configuración de la aplicación recibe automáticamente la configuración del host proporcionada por [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-458">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="9ba3e-459">Configuración de aplicación de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-459">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="9ba3e-460">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-460">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="9ba3e-461">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-461">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="9ba3e-462">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-462">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="9ba3e-463">Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-463">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="9ba3e-464">La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento `<Content>` siguiente:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-464">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="9ba3e-465">Los métodos de extensión de configuración, como <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> y <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> requieren paquetes NuGet adicionales, como [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) y [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-465">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="9ba3e-466">A menos que la aplicación use el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), además del paquete principal [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration), se deben agregar estos paquetes.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-466">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="9ba3e-467">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-467">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="9ba3e-468">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="9ba3e-468">ConfigureServices</span></span>

<span data-ttu-id="9ba3e-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9ba3e-470">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="9ba3e-471">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-471">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9ba3e-472">Para más información, consulte <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-472">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="9ba3e-473">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-473">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="9ba3e-474">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="9ba3e-474">ConfigureLogging</span></span>

<span data-ttu-id="9ba3e-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> agrega un delegado para configurar el <xref:Microsoft.Extensions.Logging.ILoggingBuilder> proporcionado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="9ba3e-476">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="9ba3e-477">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-477">UseConsoleLifetime</span></span>

<span data-ttu-id="9ba3e-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> escucha Ctrl+C/SIGINT o SIGTERM y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="9ba3e-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="9ba3e-480">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-480">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="9ba3e-481">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-481">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="9ba3e-482">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="9ba3e-482">Container configuration</span></span>

<span data-ttu-id="9ba3e-483">Para permitir la conexión a otros contenedores, el host puede aceptar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-483">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="9ba3e-484">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-484">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="9ba3e-485">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-485">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="9ba3e-486">La configuración personalizada del contenedor está administrada por el método <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-486">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="9ba3e-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="9ba3e-488">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="9ba3e-489">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-489">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="9ba3e-490">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-490">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="9ba3e-491">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-491">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="9ba3e-492">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="9ba3e-492">Extensibility</span></span>

<span data-ttu-id="9ba3e-493">La extensibilidad de host se realiza con métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-493">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="9ba3e-494">En el ejemplo siguiente se muestra cómo un método de extensión extiende una implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder> con el ejemplo [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) demostrado en <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-494">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="9ba3e-495">Una aplicación establece el método de extensión `UseHostedService` para registrar el servicio hospedado pasado en `T`:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-495">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="9ba3e-496">Administración del host</span><span class="sxs-lookup"><span data-stu-id="9ba3e-496">Manage the host</span></span>

<span data-ttu-id="9ba3e-497">La implementación de <xref:Microsoft.Extensions.Hosting.IHost> es la responsable de iniciar y detener las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-497">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="9ba3e-498">Run</span><span class="sxs-lookup"><span data-stu-id="9ba3e-498">Run</span></span>

<span data-ttu-id="9ba3e-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="9ba3e-500">RunAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-500">RunAsync</span></span>

<span data-ttu-id="9ba3e-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="9ba3e-502">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-502">RunConsoleAsync</span></span>

<span data-ttu-id="9ba3e-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre Ctrl+C/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="9ba3e-504">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-504">Start and StopAsync</span></span>

<span data-ttu-id="9ba3e-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="9ba3e-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="9ba3e-507">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-507">StartAsync and StopAsync</span></span>

<span data-ttu-id="9ba3e-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="9ba3e-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="9ba3e-510">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="9ba3e-510">WaitForShutdown</span></span>

<span data-ttu-id="9ba3e-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> se desencadena mediante <xref:Microsoft.Extensions.Hosting.IHostLifetime>, como `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (escucha Ctrl+C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="9ba3e-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="9ba3e-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="9ba3e-513">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="9ba3e-513">WaitForShutdownAsync</span></span>

<span data-ttu-id="9ba3e-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="9ba3e-515">Control externo</span><span class="sxs-lookup"><span data-stu-id="9ba3e-515">External control</span></span>

<span data-ttu-id="9ba3e-516">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-516">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="9ba3e-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="9ba3e-518">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-518">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9ba3e-519">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="9ba3e-519">IHostingEnvironment interface</span></span>

<span data-ttu-id="9ba3e-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="9ba3e-521">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-521">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="9ba3e-522">Para más información, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-522">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9ba3e-523">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9ba3e-523">IApplicationLifetime interface</span></span>

<span data-ttu-id="9ba3e-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="9ba3e-525">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos <xref:System.Action> que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-525">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="9ba3e-526">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="9ba3e-526">Cancellation Token</span></span> | <span data-ttu-id="9ba3e-527">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="9ba3e-527">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="9ba3e-528">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-528">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="9ba3e-529">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-529">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9ba3e-530">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-530">All requests should be processed.</span></span> <span data-ttu-id="9ba3e-531">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-531">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="9ba3e-532">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-532">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9ba3e-533">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-533">Requests may still be processing.</span></span> <span data-ttu-id="9ba3e-534">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-534">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="9ba3e-535">Inserción de constructor del servicio <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-535">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="9ba3e-536">En la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de <xref:Microsoft.Extensions.Hosting.IHostedService>) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-536">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="9ba3e-537">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-537">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="9ba3e-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9ba3e-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="9ba3e-539">La siguiente clase usa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="9ba3e-539">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9ba3e-540">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9ba3e-540">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
