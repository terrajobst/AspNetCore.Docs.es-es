---
title: Host genérico de .NET
author: rick-anderson
description: Obtenga información sobre el host genérico .NET Core, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
uid: fundamentals/host/generic-host
ms.openlocfilehash: 454216cec72048217ede412f8ff6d4261f7353b1
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417634"
---
# <a name="net-generic-host"></a><span data-ttu-id="2ec67-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="2ec67-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="2ec67-104">En este artículo se presenta el host genérico .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) y se proporciona orientación sobre cómo usarlo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="2ec67-105">¿Qué es un host?</span><span class="sxs-lookup"><span data-stu-id="2ec67-105">What's a host?</span></span>

<span data-ttu-id="2ec67-106">El *host* es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="2ec67-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="2ec67-107">Inserción de dependencias (ID)</span><span class="sxs-lookup"><span data-stu-id="2ec67-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="2ec67-108">Registro</span><span class="sxs-lookup"><span data-stu-id="2ec67-108">Logging</span></span>
* <span data-ttu-id="2ec67-109">Configuración</span><span class="sxs-lookup"><span data-stu-id="2ec67-109">Configuration</span></span>
* <span data-ttu-id="2ec67-110">Implementaciones de `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="2ec67-110">`IHostedService` implementations</span></span>

<span data-ttu-id="2ec67-111">Cuando se inicia un host, llama a `IHostedService.StartAsync` en cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> que encuentra en el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="2ec67-112">En una aplicación web, una de las implementaciones de `IHostedService` es un servicio web que inicia una [implementación de servidor HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="2ec67-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="2ec67-113">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="2ec67-114">En las versiones de ASP.NET Core anteriores a la 3.0, el [host web](xref:fundamentals/host/web-host) se usa para cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="2ec67-115">El host web ya no se recomienda para las aplicaciones web, pero sigue estando disponible para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="2ec67-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="2ec67-116">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="2ec67-116">Set up a host</span></span>

<span data-ttu-id="2ec67-117">Normalmente se configura, compila y ejecuta el host por el código de la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="2ec67-118">El método `Main` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-118">The `Main` method:</span></span>

* <span data-ttu-id="2ec67-119">Llama a un método `CreateHostBuilder` para crear y configurar un objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="2ec67-120">Llama a los métodos `Build` y `Run` en el objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="2ec67-121">Este es el código de *Program.cs* para una carga de trabajo no HTTP, con una sola implementación de `IHostedService` agregada para el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="2ec67-122">Para una carga de trabajo HTTP, el método `Main` es el mismo, pero `CreateHostBuilder` llama a `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="2ec67-123">Si la aplicación usa Entity Framework Core, no cambie el nombre o la firma del método `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="2ec67-124">Las [herramientas de Entity Framework Core](/ef/core/miscellaneous/cli/) esperan encontrar un método `CreateHostBuilder` que configure el host sin ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="2ec67-125">Para obtener más información, consulte [Creación de DbContext en tiempo de diseño](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="2ec67-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="2ec67-126">Configuración predeterminada del generador</span><span class="sxs-lookup"><span data-stu-id="2ec67-126">Default builder settings</span></span>

<span data-ttu-id="2ec67-127">El método <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="2ec67-128">Establece la [raíz de contenido](xref:fundamentals/index#content-root) en la ruta de acceso devuelta por <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="2ec67-129">Carga la configuración de host de:</span><span class="sxs-lookup"><span data-stu-id="2ec67-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="2ec67-130">Variables de entorno con el prefijo `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-130">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="2ec67-131">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-131">Command-line arguments.</span></span>
* <span data-ttu-id="2ec67-132">Carga la configuración de aplicación de:</span><span class="sxs-lookup"><span data-stu-id="2ec67-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="2ec67-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="2ec67-134">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="2ec67-135">[Administrador de secretos](xref:security/app-secrets), cuando la aplicación se ejecuta en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="2ec67-136">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-136">Environment variables.</span></span>
  * <span data-ttu-id="2ec67-137">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-137">Command-line arguments.</span></span>
* <span data-ttu-id="2ec67-138">Agrega los siguientes proveedores de [registro](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="2ec67-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="2ec67-139">Consola</span><span class="sxs-lookup"><span data-stu-id="2ec67-139">Console</span></span>
  * <span data-ttu-id="2ec67-140">Depuración</span><span class="sxs-lookup"><span data-stu-id="2ec67-140">Debug</span></span>
  * <span data-ttu-id="2ec67-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="2ec67-141">EventSource</span></span>
  * <span data-ttu-id="2ec67-142">EventLog (solo si se ejecuta en Windows)</span><span class="sxs-lookup"><span data-stu-id="2ec67-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="2ec67-143">Permite la [validación del ámbito](xref:fundamentals/dependency-injection#scope-validation) y la [validación de dependencias](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) si el entorno es Desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="2ec67-144">El método `ConfigureWebHostDefaults` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="2ec67-145">Carga la configuración de host desde las variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-145">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="2ec67-146">Establece el servidor [Kestrel](xref:fundamentals/servers/kestrel) como servidor web y lo configura por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="2ec67-147">Para conocer las opciones predeterminadas del servidor Kestrel, consulte <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="2ec67-148">Agrega el [middleware de filtrado de hosts](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="2ec67-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="2ec67-149">Agrega [middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) si `ASPNETCORE_FORWARDEDHEADERS_ENABLED` es igual a `true`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="2ec67-150">Permite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="2ec67-150">Enables IIS integration.</span></span> <span data-ttu-id="2ec67-151">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="2ec67-152">En las secciones [Configuración para todos los tipos de aplicaciones](#settings-for-all-app-types) y [Configuración para las aplicaciones web](#settings-for-web-apps), más adelante en este artículo, se muestra cómo invalidar la configuración predeterminada del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="2ec67-153">Servicios proporcionados por el marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="2ec67-153">Framework-provided services</span></span>

<span data-ttu-id="2ec67-154">Los servicios siguientes se registran de forma automática:</span><span class="sxs-lookup"><span data-stu-id="2ec67-154">The following services are registered automatically:</span></span>

* [<span data-ttu-id="2ec67-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="2ec67-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="2ec67-157">IHostEnvironment/IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="2ec67-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="2ec67-158">Para más información sobre los servicios proporcionados por el marco, consulte <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="2ec67-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="2ec67-160">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (anteriormente, `IApplicationLifetime`) en cualquier clase para controlar las tareas posteriores al inicio y el cierre estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="2ec67-161">Tres de las propiedades de la interfaz son tokens de cancelación que se usan para registrar los métodos del controlador de eventos de inicio y detención de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2ec67-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="2ec67-162">La interfaz también incluye un método `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="2ec67-163">El ejemplo siguiente es una implementación de `IHostedService` que registra los eventos `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="2ec67-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-164">IHostLifetime</span></span>

<span data-ttu-id="2ec67-165">La implementación de <xref:Microsoft.Extensions.Hosting.IHostLifetime> controla cuándo se inicia el host y cuándo se detiene.</span><span class="sxs-lookup"><span data-stu-id="2ec67-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="2ec67-166">Se usa la última implementación registrada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-166">The last implementation registered is used.</span></span>

<span data-ttu-id="2ec67-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` es la implementación predeterminada de `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="2ec67-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="2ec67-169">escucha <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM, y llama a <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> para iniciar el proceso de apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-169">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="2ec67-170">Desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="2ec67-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="2ec67-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="2ec67-171">IHostEnvironment</span></span>

<span data-ttu-id="2ec67-172">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostEnvironment> en una clase para obtener información sobre los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="2ec67-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="2ec67-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="2ec67-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="2ec67-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="2ec67-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="2ec67-176">Las aplicaciones web implementan la interfaz `IWebHostEnvironment`, que hereda `IHostEnvironment` y agrega [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="2ec67-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="2ec67-177">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="2ec67-177">Host configuration</span></span>

<span data-ttu-id="2ec67-178">La configuración de host se usa para las propiedades de la implementación de <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="2ec67-179">La configuración de host está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration), en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="2ec67-180">Después de `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` se reemplaza por la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="2ec67-181">Para agregar la configuración de host, llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="2ec67-182">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-183">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="2ec67-184">El proveedor de variables de entorno con el prefijo `DOTNET_` y los argumentos de línea de comandos se incluyen mediante `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-184">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ec67-185">Para las aplicaciones web, se agrega el proveedor de variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="2ec67-186">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="2ec67-187">Por ejemplo, el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="2ec67-188">En el ejemplo siguiente se crea la configuración de host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="2ec67-189">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="2ec67-189">App configuration</span></span>

<span data-ttu-id="2ec67-190">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="2ec67-191">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-192">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="2ec67-193">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y como servicio de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="2ec67-194">La configuración de host también se agrega a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="2ec67-195">Para más información, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="2ec67-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="2ec67-196">Configuración para todos los tipos de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="2ec67-196">Settings for all app types</span></span>

<span data-ttu-id="2ec67-197">En esta sección se enumeran las configuraciones de host que se aplican a las cargas de trabajo HTTP y no HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="2ec67-198">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="2ec67-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="2ec67-199">ApplicationName</span></span>

<span data-ttu-id="2ec67-200">La propiedad [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) se establece desde la configuración de host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="2ec67-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="2ec67-201">**Clave**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="2ec67-201">**Key**: `applicationName`</span></span>  
<span data-ttu-id="2ec67-202">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-202">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-203">**Predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="2ec67-204">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="2ec67-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="2ec67-205">Para establecer este valor, use la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="2ec67-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-206">ContentRootPath</span></span>

<span data-ttu-id="2ec67-207">La propiedad [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="2ec67-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="2ec67-208">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="2ec67-209">**Clave**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-209">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="2ec67-210">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-210">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-211">**Predeterminado**: carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="2ec67-212">**Variable de entorno**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="2ec67-213">Para establecer este valor, use la variable de entorno o llame a `UseContentRoot` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="2ec67-214">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="2ec67-214">For more information, see:</span></span>

* [<span data-ttu-id="2ec67-215">Aspectos básicos: Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="2ec67-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="2ec67-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="2ec67-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="2ec67-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="2ec67-217">EnvironmentName</span></span>

<span data-ttu-id="2ec67-218">La propiedad [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) puede establecerse en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="2ec67-219">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2ec67-220">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2ec67-220">Values aren't case-sensitive.</span></span>

<span data-ttu-id="2ec67-221">**Clave**: `environment`</span><span class="sxs-lookup"><span data-stu-id="2ec67-221">**Key**: `environment`</span></span>  
<span data-ttu-id="2ec67-222">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-222">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-223">**Predeterminado**: `Production`</span><span class="sxs-lookup"><span data-stu-id="2ec67-223">**Default**: `Production`</span></span>  
<span data-ttu-id="2ec67-224">**Variable de entorno**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="2ec67-225">Para establecer este valor, use la variable de entorno o llame a `UseEnvironment` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="2ec67-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="2ec67-226">ShutdownTimeout</span></span>

<span data-ttu-id="2ec67-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="2ec67-228">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-228">The default value is five seconds.</span></span>  <span data-ttu-id="2ec67-229">Durante el período de tiempo de espera, el host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="2ec67-230">Activa [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="2ec67-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="2ec67-231">Trata de detener los servicios hospedados y registra los errores que se producen en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="2ec67-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="2ec67-232">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="2ec67-233">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="2ec67-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="2ec67-234">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="2ec67-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="2ec67-235">**Clave**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="2ec67-235">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="2ec67-236">**Tipo**: `int`</span><span class="sxs-lookup"><span data-stu-id="2ec67-236">**Type**: `int`</span></span>  
<span data-ttu-id="2ec67-237">**Predeterminado**: 5 segundos</span><span class="sxs-lookup"><span data-stu-id="2ec67-237">**Default**: 5 seconds</span></span>  
<span data-ttu-id="2ec67-238">**Variable de entorno**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-238">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="2ec67-239">Para establecer este valor, use la variable de entorno o configure `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="2ec67-240">El siguiente ejemplo establece el tiempo de espera en 20 segundos:</span><span class="sxs-lookup"><span data-stu-id="2ec67-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a><span data-ttu-id="2ec67-241">Deshabilitación de la recarga de configuración de aplicaciones al realizar un cambio</span><span class="sxs-lookup"><span data-stu-id="2ec67-241">Disable app configuration reload on change</span></span>

<span data-ttu-id="2ec67-242">[De forma predeterminada](xref:fundamentals/configuration/index#default), *appsettings.json* y *appsettings.{Entorno}.json* se recargan cuando el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="2ec67-242">By [default](xref:fundamentals/configuration/index#default), *appsettings.json* and *appsettings.{Environment}.json* are reloaded when the file changes.</span></span> <span data-ttu-id="2ec67-243">Para deshabilitar este comportamiento de recarga en ASP.NET Core 5.0 Preview 3 o versiones posteriores, establezca la clave `hostBuilder:reloadConfigOnChange` en `false`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-243">To disable this reload behavior in ASP.NET Core 5.0 Preview 3 or later, set the `hostBuilder:reloadConfigOnChange` key to `false`.</span></span>

<span data-ttu-id="2ec67-244">**Clave**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="2ec67-244">**Key**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="2ec67-245">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-245">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-246">**Predeterminado**: `true`</span><span class="sxs-lookup"><span data-stu-id="2ec67-246">**Default**: `true`</span></span>  
<span data-ttu-id="2ec67-247">**Argumento de la línea de comandos**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="2ec67-247">**Command-line argument**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="2ec67-248">**Variable de entorno**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="2ec67-248">**Environment variable**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span></span>

> [!WARNING]
> <span data-ttu-id="2ec67-249">El separador de dos puntos (`:`) no funciona con las claves jerárquicas de variables de entorno en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="2ec67-249">The colon (`:`) separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="2ec67-250">Para más información, consulte [Variables de entorno](xref:fundamentals/configuration/index#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="2ec67-250">For more information, see [Environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span>

## <a name="settings-for-web-apps"></a><span data-ttu-id="2ec67-251">Configuración para las aplicaciones web</span><span class="sxs-lookup"><span data-stu-id="2ec67-251">Settings for web apps</span></span>

<span data-ttu-id="2ec67-252">Algunas configuraciones de host solo se aplican a las cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-252">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="2ec67-253">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-253">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="2ec67-254">Los métodos de extensión en `IWebHostBuilder` están disponibles para estas configuraciones.</span><span class="sxs-lookup"><span data-stu-id="2ec67-254">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="2ec67-255">Los ejemplos de código que muestran cómo llamar a los métodos de extensión suponen que `webBuilder` es una instancia de `IWebHostBuilder`, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2ec67-255">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="2ec67-256">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="2ec67-256">CaptureStartupErrors</span></span>

<span data-ttu-id="2ec67-257">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-257">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="2ec67-258">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-258">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="2ec67-259">**Clave**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="2ec67-259">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="2ec67-260">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-260">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-261">**Predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-261">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="2ec67-262">**Variable de entorno**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-262">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="2ec67-263">Para establecer este valor, utilice la configuración o llame a `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-263">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="2ec67-264">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="2ec67-264">DetailedErrors</span></span>

<span data-ttu-id="2ec67-265">Si se habilita, o si el entorno es `Development`, la aplicación captura errores detallados.</span><span class="sxs-lookup"><span data-stu-id="2ec67-265">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="2ec67-266">**Clave**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="2ec67-266">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="2ec67-267">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-267">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-268">**Predeterminado**: `false`</span><span class="sxs-lookup"><span data-stu-id="2ec67-268">**Default**: `false`</span></span>  
<span data-ttu-id="2ec67-269">**Variable de entorno**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-269">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="2ec67-270">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-270">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="2ec67-271">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="2ec67-271">HostingStartupAssemblies</span></span>

<span data-ttu-id="2ec67-272">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-272">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="2ec67-273">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-273">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="2ec67-274">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-274">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="2ec67-275">**Clave**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="2ec67-275">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="2ec67-276">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-276">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-277">**Predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="2ec67-277">**Default**: Empty string</span></span>  
<span data-ttu-id="2ec67-278">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2ec67-278">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="2ec67-279">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="2ec67-280">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="2ec67-280">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="2ec67-281">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-281">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="2ec67-282">**Clave**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="2ec67-282">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="2ec67-283">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-283">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-284">**Predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="2ec67-284">**Default**: Empty string</span></span>  
<span data-ttu-id="2ec67-285">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2ec67-285">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="2ec67-286">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-286">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="2ec67-287">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="2ec67-287">HTTPS_Port</span></span>

<span data-ttu-id="2ec67-288">Puerto de redireccionamiento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ec67-288">The HTTPS redirect port.</span></span> <span data-ttu-id="2ec67-289">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2ec67-289">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="2ec67-290">**Clave**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="2ec67-290">**Key**: `https_port`</span></span>  
<span data-ttu-id="2ec67-291">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-291">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-292">**Predeterminado**: no se ha establecido ningún valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-292">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="2ec67-293">**Variable de entorno**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-293">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="2ec67-294">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-294">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="2ec67-295">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="2ec67-295">PreferHostingUrls</span></span>

<span data-ttu-id="2ec67-296">Indica si el host debe escuchar en las direcciones URL configuradas con `IWebHostBuilder` en lugar de las que están configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-296">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="2ec67-297">**Clave**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="2ec67-297">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="2ec67-298">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-298">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-299">**Predeterminado**: `true`</span><span class="sxs-lookup"><span data-stu-id="2ec67-299">**Default**: `true`</span></span>  
<span data-ttu-id="2ec67-300">**Variable de entorno**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-300">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="2ec67-301">Para establecer este valor, use la variable de entorno o llame a `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-301">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="2ec67-302">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="2ec67-302">PreventHostingStartup</span></span>

<span data-ttu-id="2ec67-303">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-303">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="2ec67-304">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-304">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="2ec67-305">**Clave**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="2ec67-305">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="2ec67-306">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-306">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-307">**Predeterminado**: `false`</span><span class="sxs-lookup"><span data-stu-id="2ec67-307">**Default**: `false`</span></span>  
<span data-ttu-id="2ec67-308">**Variable de entorno**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="2ec67-308">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="2ec67-309">Para establecer este valor, use la variable de entorno o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-309">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="2ec67-310">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="2ec67-310">StartupAssembly</span></span>

<span data-ttu-id="2ec67-311">Ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-311">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="2ec67-312">**Clave**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="2ec67-312">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="2ec67-313">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-313">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-314">**Predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2ec67-314">**Default**: The app's assembly</span></span>  
<span data-ttu-id="2ec67-315">**Variable de entorno**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="2ec67-315">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="2ec67-316">Para establecer este valor, use la variable de entorno o llame a `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-316">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="2ec67-317">`UseStartup` puede tomar un nombre del ensamblado (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="2ec67-317">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="2ec67-318">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="2ec67-318">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="2ec67-319">Direcciones URL</span><span class="sxs-lookup"><span data-stu-id="2ec67-319">URLs</span></span>

<span data-ttu-id="2ec67-320">Lista delimitada por punto y coma de las direcciones IP o las direcciones de host con los puertos y protocolos en los que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2ec67-320">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="2ec67-321">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-321">For example, `http://localhost:123`.</span></span> <span data-ttu-id="2ec67-322">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="2ec67-322">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="2ec67-323">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="2ec67-323">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="2ec67-324">Los formatos admitidos varían de un servidor a otro.</span><span class="sxs-lookup"><span data-stu-id="2ec67-324">Supported formats vary among servers.</span></span>

<span data-ttu-id="2ec67-325">**Clave**: `urls`</span><span class="sxs-lookup"><span data-stu-id="2ec67-325">**Key**: `urls`</span></span>  
<span data-ttu-id="2ec67-326">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-326">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-327">**Predeterminado**: `http://localhost:5000` y `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="2ec67-327">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="2ec67-328">**Variable de entorno**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-328">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="2ec67-329">Para establecer este valor, use la variable de entorno o llame a `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-329">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="2ec67-330">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="2ec67-330">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="2ec67-331">Para obtener más información, vea <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-331">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="2ec67-332">WebRoot</span><span class="sxs-lookup"><span data-stu-id="2ec67-332">WebRoot</span></span>

<span data-ttu-id="2ec67-333">Ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-333">The relative path to the app's static assets.</span></span>

<span data-ttu-id="2ec67-334">**Clave**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-334">**Key**: `webroot`</span></span>  
<span data-ttu-id="2ec67-335">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-335">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-336">**Predeterminado**: De manera predeterminada, es `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-336">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="2ec67-337">Debe existir la ruta de acceso a *{raíz del contenido}/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-337">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="2ec67-338">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="2ec67-338">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="2ec67-339">**Variable de entorno**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-339">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="2ec67-340">Para establecer este valor, use la variable de entorno o llame a `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-340">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="2ec67-341">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="2ec67-341">For more information, see:</span></span>

* [<span data-ttu-id="2ec67-342">Aspectos básicos: Raíz web</span><span class="sxs-lookup"><span data-stu-id="2ec67-342">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="2ec67-343">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-343">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="2ec67-344">Administración de la vigencia del host</span><span class="sxs-lookup"><span data-stu-id="2ec67-344">Manage the host lifetime</span></span>

<span data-ttu-id="2ec67-345">Llame a los métodos en la implementación de <xref:Microsoft.Extensions.Hosting.IHost> creada para iniciar y detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-345">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="2ec67-346">Estos métodos afectan a todas las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="2ec67-346">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="2ec67-347">Run</span><span class="sxs-lookup"><span data-stu-id="2ec67-347">Run</span></span>

<span data-ttu-id="2ec67-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="2ec67-349">RunAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-349">RunAsync</span></span>

<span data-ttu-id="2ec67-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="2ec67-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="2ec67-351">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-351">RunConsoleAsync</span></span>

<span data-ttu-id="2ec67-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host, y espera a <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM para el apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="2ec67-353">Iniciar</span><span class="sxs-lookup"><span data-stu-id="2ec67-353">Start</span></span>

<span data-ttu-id="2ec67-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="2ec67-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="2ec67-355">StartAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-355">StartAsync</span></span>

<span data-ttu-id="2ec67-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia el host y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="2ec67-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="2ec67-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de `StartAsync`, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="2ec67-358">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-358">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="2ec67-359">StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-359">StopAsync</span></span>

<span data-ttu-id="2ec67-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="2ec67-361">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="2ec67-361">WaitForShutdown</span></span>

<span data-ttu-id="2ec67-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> bloquea el subproceso de llamada hasta que IHostLifetime desencadena el apagado, por ejemplo, a través de <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="2ec67-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="2ec67-363">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-363">WaitForShutdownAsync</span></span>

<span data-ttu-id="2ec67-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="2ec67-365">Control externo</span><span class="sxs-lookup"><span data-stu-id="2ec67-365">External control</span></span>

<span data-ttu-id="2ec67-366">El control directo de la vigencia del host se puede lograr mediante métodos a los que se puede llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="2ec67-366">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

<span data-ttu-id="2ec67-367">En este artículo se presenta el host genérico .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) y se proporciona orientación sobre cómo usarlo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-367">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="2ec67-368">¿Qué es un host?</span><span class="sxs-lookup"><span data-stu-id="2ec67-368">What's a host?</span></span>

<span data-ttu-id="2ec67-369">El *host* es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="2ec67-369">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="2ec67-370">Inserción de dependencias (ID)</span><span class="sxs-lookup"><span data-stu-id="2ec67-370">Dependency injection (DI)</span></span>
* <span data-ttu-id="2ec67-371">Registro</span><span class="sxs-lookup"><span data-stu-id="2ec67-371">Logging</span></span>
* <span data-ttu-id="2ec67-372">Configuración</span><span class="sxs-lookup"><span data-stu-id="2ec67-372">Configuration</span></span>
* <span data-ttu-id="2ec67-373">Implementaciones de `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="2ec67-373">`IHostedService` implementations</span></span>

<span data-ttu-id="2ec67-374">Cuando se inicia un host, llama a `IHostedService.StartAsync` en cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> que encuentra en el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-374">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="2ec67-375">En una aplicación web, una de las implementaciones de `IHostedService` es un servicio web que inicia una [implementación de servidor HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="2ec67-375">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="2ec67-376">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-376">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="2ec67-377">En las versiones de ASP.NET Core anteriores a la 3.0, el [host web](xref:fundamentals/host/web-host) se usa para cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-377">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="2ec67-378">El host web ya no se recomienda para las aplicaciones web, pero sigue estando disponible para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="2ec67-378">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="2ec67-379">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="2ec67-379">Set up a host</span></span>

<span data-ttu-id="2ec67-380">Normalmente se configura, compila y ejecuta el host por el código de la clase `Program`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-380">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="2ec67-381">El método `Main` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-381">The `Main` method:</span></span>

* <span data-ttu-id="2ec67-382">Llama a un método `CreateHostBuilder` para crear y configurar un objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-382">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="2ec67-383">Llama a los métodos `Build` y `Run` en el objeto del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-383">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="2ec67-384">Este es el código de *Program.cs* para una carga de trabajo no HTTP, con una sola implementación de `IHostedService` agregada para el contenedor de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-384">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="2ec67-385">Para una carga de trabajo HTTP, el método `Main` es el mismo, pero `CreateHostBuilder` llama a `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-385">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="2ec67-386">Si la aplicación usa Entity Framework Core, no cambie el nombre o la firma del método `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-386">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="2ec67-387">Las [herramientas de Entity Framework Core](/ef/core/miscellaneous/cli/) esperan encontrar un método `CreateHostBuilder` que configure el host sin ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-387">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="2ec67-388">Para obtener más información, consulte [Creación de DbContext en tiempo de diseño](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="2ec67-388">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="2ec67-389">Configuración predeterminada del generador</span><span class="sxs-lookup"><span data-stu-id="2ec67-389">Default builder settings</span></span>

<span data-ttu-id="2ec67-390">El método <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-390">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="2ec67-391">Establece la [raíz de contenido](xref:fundamentals/index#content-root) en la ruta de acceso devuelta por <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-391">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="2ec67-392">Carga la configuración de host de:</span><span class="sxs-lookup"><span data-stu-id="2ec67-392">Loads host configuration from:</span></span>
  * <span data-ttu-id="2ec67-393">Variables de entorno con el prefijo `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-393">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="2ec67-394">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-394">Command-line arguments.</span></span>
* <span data-ttu-id="2ec67-395">Carga la configuración de aplicación de:</span><span class="sxs-lookup"><span data-stu-id="2ec67-395">Loads app configuration from:</span></span>
  * <span data-ttu-id="2ec67-396">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-396">*appsettings.json*.</span></span>
  * <span data-ttu-id="2ec67-397">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-397">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="2ec67-398">[Administrador de secretos](xref:security/app-secrets), cuando la aplicación se ejecuta en el entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-398">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="2ec67-399">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-399">Environment variables.</span></span>
  * <span data-ttu-id="2ec67-400">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-400">Command-line arguments.</span></span>
* <span data-ttu-id="2ec67-401">Agrega los siguientes proveedores de [registro](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="2ec67-401">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="2ec67-402">Consola</span><span class="sxs-lookup"><span data-stu-id="2ec67-402">Console</span></span>
  * <span data-ttu-id="2ec67-403">Depuración</span><span class="sxs-lookup"><span data-stu-id="2ec67-403">Debug</span></span>
  * <span data-ttu-id="2ec67-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="2ec67-404">EventSource</span></span>
  * <span data-ttu-id="2ec67-405">EventLog (solo si se ejecuta en Windows)</span><span class="sxs-lookup"><span data-stu-id="2ec67-405">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="2ec67-406">Permite la [validación del ámbito](xref:fundamentals/dependency-injection#scope-validation) y la [validación de dependencias](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) si el entorno es Desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-406">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="2ec67-407">El método `ConfigureWebHostDefaults` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-407">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="2ec67-408">Carga la configuración de host desde las variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-408">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="2ec67-409">Establece el servidor [Kestrel](xref:fundamentals/servers/kestrel) como servidor web y lo configura por medio de los proveedores de configuración de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-409">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="2ec67-410">Para conocer las opciones predeterminadas del servidor Kestrel, consulte <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-410">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="2ec67-411">Agrega el [middleware de filtrado de hosts](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="2ec67-411">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="2ec67-412">Agrega [middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) si `ASPNETCORE_FORWARDEDHEADERS_ENABLED` es igual a `true`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-412">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="2ec67-413">Permite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="2ec67-413">Enables IIS integration.</span></span> <span data-ttu-id="2ec67-414">Para consultar las opciones predeterminadas de IIS, vea <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-414">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="2ec67-415">En las secciones [Configuración para todos los tipos de aplicaciones](#settings-for-all-app-types) y [Configuración para las aplicaciones web](#settings-for-web-apps), más adelante en este artículo, se muestra cómo invalidar la configuración predeterminada del generador.</span><span class="sxs-lookup"><span data-stu-id="2ec67-415">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="2ec67-416">Servicios proporcionados por el marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="2ec67-416">Framework-provided services</span></span>

<span data-ttu-id="2ec67-417">Los servicios siguientes se registran de forma automática:</span><span class="sxs-lookup"><span data-stu-id="2ec67-417">The following services are registered automatically:</span></span>

* [<span data-ttu-id="2ec67-418">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-418">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="2ec67-419">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-419">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="2ec67-420">IHostEnvironment/IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="2ec67-420">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="2ec67-421">Para más información sobre los servicios proporcionados por el marco, consulte <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-421">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="2ec67-422">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-422">IHostApplicationLifetime</span></span>

<span data-ttu-id="2ec67-423">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (anteriormente, `IApplicationLifetime`) en cualquier clase para controlar las tareas posteriores al inicio y el cierre estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-423">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="2ec67-424">Tres de las propiedades de la interfaz son tokens de cancelación que se usan para registrar los métodos del controlador de eventos de inicio y detención de las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2ec67-424">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="2ec67-425">La interfaz también incluye un método `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-425">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="2ec67-426">El ejemplo siguiente es una implementación de `IHostedService` que registra los eventos `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-426">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="2ec67-427">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-427">IHostLifetime</span></span>

<span data-ttu-id="2ec67-428">La implementación de <xref:Microsoft.Extensions.Hosting.IHostLifetime> controla cuándo se inicia el host y cuándo se detiene.</span><span class="sxs-lookup"><span data-stu-id="2ec67-428">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="2ec67-429">Se usa la última implementación registrada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-429">The last implementation registered is used.</span></span>

<span data-ttu-id="2ec67-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` es la implementación predeterminada de `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="2ec67-431">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-431">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="2ec67-432">escucha <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM, y llama a <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> para iniciar el proceso de apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-432">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="2ec67-433">Desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="2ec67-433">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="2ec67-434">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="2ec67-434">IHostEnvironment</span></span>

<span data-ttu-id="2ec67-435">Permite insertar el servicio <xref:Microsoft.Extensions.Hosting.IHostEnvironment> en una clase para obtener información sobre los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ec67-435">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="2ec67-436">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="2ec67-436">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="2ec67-437">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="2ec67-437">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="2ec67-438">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-438">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="2ec67-439">Las aplicaciones web implementan la interfaz `IWebHostEnvironment`, que hereda `IHostEnvironment` y agrega [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="2ec67-439">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="2ec67-440">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="2ec67-440">Host configuration</span></span>

<span data-ttu-id="2ec67-441">La configuración de host se usa para las propiedades de la implementación de <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-441">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="2ec67-442">La configuración de host está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration), en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-442">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="2ec67-443">Después de `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` se reemplaza por la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-443">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="2ec67-444">Para agregar la configuración de host, llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-444">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="2ec67-445">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-445">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-446">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-446">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="2ec67-447">El proveedor de variables de entorno con el prefijo `DOTNET_` y los argumentos de línea de comandos se incluyen mediante `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-447">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="2ec67-448">Para las aplicaciones web, se agrega el proveedor de variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-448">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="2ec67-449">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-449">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="2ec67-450">Por ejemplo, el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-450">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="2ec67-451">En el ejemplo siguiente se crea la configuración de host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-451">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="2ec67-452">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="2ec67-452">App configuration</span></span>

<span data-ttu-id="2ec67-453">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="2ec67-454">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-454">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-455">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-455">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="2ec67-456">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y como servicio de ID.</span><span class="sxs-lookup"><span data-stu-id="2ec67-456">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="2ec67-457">La configuración de host también se agrega a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-457">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="2ec67-458">Para más información, consulte [Configuración en ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="2ec67-458">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="2ec67-459">Configuración para todos los tipos de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="2ec67-459">Settings for all app types</span></span>

<span data-ttu-id="2ec67-460">En esta sección se enumeran las configuraciones de host que se aplican a las cargas de trabajo HTTP y no HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-460">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="2ec67-461">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-461">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="2ec67-462">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="2ec67-462">ApplicationName</span></span>

<span data-ttu-id="2ec67-463">La propiedad [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) se establece desde la configuración de host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="2ec67-463">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="2ec67-464">**Clave**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="2ec67-464">**Key**: `applicationName`</span></span>  
<span data-ttu-id="2ec67-465">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-465">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-466">**Predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-466">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="2ec67-467">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="2ec67-467">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="2ec67-468">Para establecer este valor, use la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-468">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="2ec67-469">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-469">ContentRootPath</span></span>

<span data-ttu-id="2ec67-470">La propiedad [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="2ec67-470">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="2ec67-471">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-471">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="2ec67-472">**Clave**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-472">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="2ec67-473">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-473">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-474">**Predeterminado**: carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-474">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="2ec67-475">**Variable de entorno**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-475">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="2ec67-476">Para establecer este valor, use la variable de entorno o llame a `UseContentRoot` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-476">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="2ec67-477">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="2ec67-477">For more information, see:</span></span>

* [<span data-ttu-id="2ec67-478">Aspectos básicos: Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="2ec67-478">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="2ec67-479">WebRoot</span><span class="sxs-lookup"><span data-stu-id="2ec67-479">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="2ec67-480">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="2ec67-480">EnvironmentName</span></span>

<span data-ttu-id="2ec67-481">La propiedad [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) puede establecerse en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-481">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="2ec67-482">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-482">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2ec67-483">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2ec67-483">Values aren't case-sensitive.</span></span>

<span data-ttu-id="2ec67-484">**Clave**: `environment`</span><span class="sxs-lookup"><span data-stu-id="2ec67-484">**Key**: `environment`</span></span>  
<span data-ttu-id="2ec67-485">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-485">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-486">**Predeterminado**: `Production`</span><span class="sxs-lookup"><span data-stu-id="2ec67-486">**Default**: `Production`</span></span>  
<span data-ttu-id="2ec67-487">**Variable de entorno**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-487">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="2ec67-488">Para establecer este valor, use la variable de entorno o llame a `UseEnvironment` en `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-488">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="2ec67-489">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="2ec67-489">ShutdownTimeout</span></span>

<span data-ttu-id="2ec67-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="2ec67-491">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-491">The default value is five seconds.</span></span>  <span data-ttu-id="2ec67-492">Durante el período de tiempo de espera, el host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-492">During the timeout period, the host:</span></span>

* <span data-ttu-id="2ec67-493">Activa [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="2ec67-493">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="2ec67-494">Trata de detener los servicios hospedados y registra los errores que se producen en los servicios que no se puedan detener.</span><span class="sxs-lookup"><span data-stu-id="2ec67-494">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="2ec67-495">Si el período de tiempo de espera expira antes de que todos los servicios hospedados se hayan detenido, los servicios activos que queden se detendrán cuando se cierre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-495">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="2ec67-496">Los servicios se detienen aun cuando no hayan terminado de procesarse.</span><span class="sxs-lookup"><span data-stu-id="2ec67-496">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="2ec67-497">Si los servicios requieren más tiempo para detenerse, aumente el valor de tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="2ec67-497">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="2ec67-498">**Clave**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="2ec67-498">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="2ec67-499">**Tipo**: `int`</span><span class="sxs-lookup"><span data-stu-id="2ec67-499">**Type**: `int`</span></span>  
<span data-ttu-id="2ec67-500">**Predeterminado**: 5 segundos</span><span class="sxs-lookup"><span data-stu-id="2ec67-500">**Default**: 5 seconds</span></span>  
<span data-ttu-id="2ec67-501">**Variable de entorno**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-501">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="2ec67-502">Para establecer este valor, use la variable de entorno o configure `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-502">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="2ec67-503">El siguiente ejemplo establece el tiempo de espera en 20 segundos:</span><span class="sxs-lookup"><span data-stu-id="2ec67-503">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="2ec67-504">Configuración para las aplicaciones web</span><span class="sxs-lookup"><span data-stu-id="2ec67-504">Settings for web apps</span></span>

<span data-ttu-id="2ec67-505">Algunas configuraciones de host solo se aplican a las cargas de trabajo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-505">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="2ec67-506">De forma predeterminada, las variables de entorno que se usan para configurar estas opciones pueden tener un prefijo `DOTNET_` o `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-506">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="2ec67-507">Los métodos de extensión en `IWebHostBuilder` están disponibles para estas configuraciones.</span><span class="sxs-lookup"><span data-stu-id="2ec67-507">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="2ec67-508">Los ejemplos de código que muestran cómo llamar a los métodos de extensión suponen que `webBuilder` es una instancia de `IWebHostBuilder`, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2ec67-508">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="2ec67-509">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="2ec67-509">CaptureStartupErrors</span></span>

<span data-ttu-id="2ec67-510">Cuando es `false`, los errores durante el inicio provocan la salida del host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-510">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="2ec67-511">Cuando es `true`, el host captura las excepciones producidas durante el inicio e intenta iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-511">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="2ec67-512">**Clave**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="2ec67-512">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="2ec67-513">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-513">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-514">**Predeterminado**: `false`, a menos que la aplicación se ejecute con Kestrel detrás de IIS, en cuyo caso el valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-514">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="2ec67-515">**Variable de entorno**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-515">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="2ec67-516">Para establecer este valor, utilice la configuración o llame a `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-516">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="2ec67-517">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="2ec67-517">DetailedErrors</span></span>

<span data-ttu-id="2ec67-518">Si se habilita, o si el entorno es `Development`, la aplicación captura errores detallados.</span><span class="sxs-lookup"><span data-stu-id="2ec67-518">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="2ec67-519">**Clave**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="2ec67-519">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="2ec67-520">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-520">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-521">**Predeterminado**: `false`</span><span class="sxs-lookup"><span data-stu-id="2ec67-521">**Default**: `false`</span></span>  
<span data-ttu-id="2ec67-522">**Variable de entorno**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-522">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="2ec67-523">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-523">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="2ec67-524">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="2ec67-524">HostingStartupAssemblies</span></span>

<span data-ttu-id="2ec67-525">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para cargar en el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-525">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="2ec67-526">Aunque el valor de configuración predeterminado es una cadena vacía, los ensamblados de inicio de hospedaje incluyen siempre el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-526">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="2ec67-527">Cuando se especifican los ensamblados de inicio de hospedaje, estos se agregan al ensamblado de la aplicación para que se carguen cuando la aplicación genera sus servicios comunes durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-527">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="2ec67-528">**Clave**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="2ec67-528">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="2ec67-529">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-529">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-530">**Predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="2ec67-530">**Default**: Empty string</span></span>  
<span data-ttu-id="2ec67-531">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2ec67-531">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="2ec67-532">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-532">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="2ec67-533">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="2ec67-533">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="2ec67-534">Una cadena delimitada por punto y coma de ensamblados de inicio de hospedaje para excluir en el inicio.</span><span class="sxs-lookup"><span data-stu-id="2ec67-534">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="2ec67-535">**Clave**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="2ec67-535">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="2ec67-536">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-536">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-537">**Predeterminado**: Cadena vacía</span><span class="sxs-lookup"><span data-stu-id="2ec67-537">**Default**: Empty string</span></span>  
<span data-ttu-id="2ec67-538">**Variable de entorno**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="2ec67-538">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="2ec67-539">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-539">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="2ec67-540">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="2ec67-540">HTTPS_Port</span></span>

<span data-ttu-id="2ec67-541">Puerto de redireccionamiento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2ec67-541">The HTTPS redirect port.</span></span> <span data-ttu-id="2ec67-542">Se usa en [Exigir HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="2ec67-542">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="2ec67-543">**Clave**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="2ec67-543">**Key**: `https_port`</span></span>  
<span data-ttu-id="2ec67-544">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-544">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-545">**Predeterminado**: no se ha establecido ningún valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-545">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="2ec67-546">**Variable de entorno**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-546">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="2ec67-547">Para establecer este valor, utilice la configuración o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-547">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="2ec67-548">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="2ec67-548">PreferHostingUrls</span></span>

<span data-ttu-id="2ec67-549">Indica si el host debe escuchar en las direcciones URL configuradas con `IWebHostBuilder` en lugar de las que están configuradas con la implementación de `IServer`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-549">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="2ec67-550">**Clave**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="2ec67-550">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="2ec67-551">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-551">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-552">**Predeterminado**: `true`</span><span class="sxs-lookup"><span data-stu-id="2ec67-552">**Default**: `true`</span></span>  
<span data-ttu-id="2ec67-553">**Variable de entorno**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-553">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="2ec67-554">Para establecer este valor, use la variable de entorno o llame a `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-554">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="2ec67-555">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="2ec67-555">PreventHostingStartup</span></span>

<span data-ttu-id="2ec67-556">Impide la carga automática de los ensamblados de inicio de hospedaje, incluidos los configurados por el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-556">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="2ec67-557">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-557">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="2ec67-558">**Clave**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="2ec67-558">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="2ec67-559">**Tipo**: `bool` (`true` o `1`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-559">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="2ec67-560">**Predeterminado**: `false`</span><span class="sxs-lookup"><span data-stu-id="2ec67-560">**Default**: `false`</span></span>  
<span data-ttu-id="2ec67-561">**Variable de entorno**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="2ec67-561">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="2ec67-562">Para establecer este valor, use la variable de entorno o llame a `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-562">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="2ec67-563">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="2ec67-563">StartupAssembly</span></span>

<span data-ttu-id="2ec67-564">Ensamblado en el que se va a buscar la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-564">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="2ec67-565">**Clave**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="2ec67-565">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="2ec67-566">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-566">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-567">**Predeterminado**: el ensamblado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2ec67-567">**Default**: The app's assembly</span></span>  
<span data-ttu-id="2ec67-568">**Variable de entorno**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="2ec67-568">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="2ec67-569">Para establecer este valor, use la variable de entorno o llame a `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-569">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="2ec67-570">`UseStartup` puede tomar un nombre del ensamblado (`string`) o un tipo (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="2ec67-570">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="2ec67-571">Si se llama a varios métodos `UseStartup`, la última llamada tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="2ec67-571">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="2ec67-572">Direcciones URL</span><span class="sxs-lookup"><span data-stu-id="2ec67-572">URLs</span></span>

<span data-ttu-id="2ec67-573">Lista delimitada por punto y coma de las direcciones IP o las direcciones de host con los puertos y protocolos en los que el servidor debe escuchar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2ec67-573">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="2ec67-574">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-574">For example, `http://localhost:123`.</span></span> <span data-ttu-id="2ec67-575">Use "\*" para indicar que el servidor debe escuchar las solicitudes en cualquier dirección IP o nombre de host con el puerto y el protocolo especificados (por ejemplo, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="2ec67-575">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="2ec67-576">El protocolo (`http://` o `https://`) debe incluirse con cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="2ec67-576">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="2ec67-577">Los formatos admitidos varían de un servidor a otro.</span><span class="sxs-lookup"><span data-stu-id="2ec67-577">Supported formats vary among servers.</span></span>

<span data-ttu-id="2ec67-578">**Clave**: `urls`</span><span class="sxs-lookup"><span data-stu-id="2ec67-578">**Key**: `urls`</span></span>  
<span data-ttu-id="2ec67-579">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-579">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-580">**Predeterminado**: `http://localhost:5000` y `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="2ec67-580">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="2ec67-581">**Variable de entorno**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="2ec67-581">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="2ec67-582">Para establecer este valor, use la variable de entorno o llame a `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-582">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="2ec67-583">Kestrel tiene su propia API de configuración de punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="2ec67-583">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="2ec67-584">Para obtener más información, vea <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-584">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="2ec67-585">WebRoot</span><span class="sxs-lookup"><span data-stu-id="2ec67-585">WebRoot</span></span>

<span data-ttu-id="2ec67-586">Ruta de acceso relativa a los recursos estáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-586">The relative path to the app's static assets.</span></span>

<span data-ttu-id="2ec67-587">**Clave**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-587">**Key**: `webroot`</span></span>  
<span data-ttu-id="2ec67-588">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-588">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-589">**Predeterminado**: De manera predeterminada, es `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-589">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="2ec67-590">Debe existir la ruta de acceso a *{raíz del contenido}/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-590">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="2ec67-591">Si la ruta de acceso no existe, se utiliza un proveedor de archivos no-op.</span><span class="sxs-lookup"><span data-stu-id="2ec67-591">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="2ec67-592">**Variable de entorno**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="2ec67-592">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="2ec67-593">Para establecer este valor, use la variable de entorno o llame a `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-593">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="2ec67-594">Para obtener más información, consulte:</span><span class="sxs-lookup"><span data-stu-id="2ec67-594">For more information, see:</span></span>

* [<span data-ttu-id="2ec67-595">Aspectos básicos: Raíz web</span><span class="sxs-lookup"><span data-stu-id="2ec67-595">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="2ec67-596">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="2ec67-596">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="2ec67-597">Administración de la vigencia del host</span><span class="sxs-lookup"><span data-stu-id="2ec67-597">Manage the host lifetime</span></span>

<span data-ttu-id="2ec67-598">Llame a los métodos en la implementación de <xref:Microsoft.Extensions.Hosting.IHost> creada para iniciar y detener la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-598">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="2ec67-599">Estos métodos afectan a todas las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="2ec67-599">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="2ec67-600">Run</span><span class="sxs-lookup"><span data-stu-id="2ec67-600">Run</span></span>

<span data-ttu-id="2ec67-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="2ec67-602">RunAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-602">RunAsync</span></span>

<span data-ttu-id="2ec67-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="2ec67-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="2ec67-604">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-604">RunConsoleAsync</span></span>

<span data-ttu-id="2ec67-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host, y espera a <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM para el apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="2ec67-606">Iniciar</span><span class="sxs-lookup"><span data-stu-id="2ec67-606">Start</span></span>

<span data-ttu-id="2ec67-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="2ec67-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="2ec67-608">StartAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-608">StartAsync</span></span>

<span data-ttu-id="2ec67-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia el host y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre.</span><span class="sxs-lookup"><span data-stu-id="2ec67-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="2ec67-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de `StartAsync`, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="2ec67-611">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-611">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="2ec67-612">StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-612">StopAsync</span></span>

<span data-ttu-id="2ec67-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="2ec67-614">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="2ec67-614">WaitForShutdown</span></span>

<span data-ttu-id="2ec67-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> bloquea el subproceso de llamada hasta que IHostLifetime desencadena el apagado, por ejemplo, a través de <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="2ec67-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="2ec67-616">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-616">WaitForShutdownAsync</span></span>

<span data-ttu-id="2ec67-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="2ec67-618">Control externo</span><span class="sxs-lookup"><span data-stu-id="2ec67-618">External control</span></span>

<span data-ttu-id="2ec67-619">El control directo de la vigencia del host se puede lograr mediante métodos a los que se puede llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="2ec67-619">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="2ec67-620">Las aplicaciones ASP.NET Core configuran e inician un host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-620">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="2ec67-621">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-621">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="2ec67-622">En este artículo se trata el host genérico de ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), que se usa para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ec67-622">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="2ec67-623">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una mayor variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-623">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="2ec67-624">La mensajería, las tareas en segundo plano y otras cargas de trabajo que no son de HTTP basadas en la ventaja de host genérico se benefician de funcionalidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="2ec67-624">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="2ec67-625">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="2ec67-625">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="2ec67-626">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2ec67-626">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="2ec67-627">El host genérico reemplazará al host web en una próxima versión y actuará como la API de host principal tanto en escenarios de HTTP como los que no lo son.</span><span class="sxs-lookup"><span data-stu-id="2ec67-627">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="2ec67-628">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ec67-628">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2ec67-629">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-629">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="2ec67-630">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-630">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="2ec67-631">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="2ec67-631">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="2ec67-632">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-632">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="2ec67-633">En la configuración de **.NET Core Launch (console)** , busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="2ec67-633">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="2ec67-634">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-634">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="2ec67-635">Introducción</span><span class="sxs-lookup"><span data-stu-id="2ec67-635">Introduction</span></span>

<span data-ttu-id="2ec67-636">La biblioteca de host genérico está disponible en el espacio de nombres <xref:Microsoft.Extensions.Hosting> y la proporciona el paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="2ec67-636">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="2ec67-637">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="2ec67-637">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="2ec67-638"><xref:Microsoft.Extensions.Hosting.IHostedService> es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="2ec67-638"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="2ec67-639">Cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="2ec67-639">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="2ec67-640">Se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> en cada <xref:Microsoft.Extensions.Hosting.IHostedService> cuando se inicia el host, y se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="2ec67-641">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="2ec67-641">Set up a host</span></span>

<span data-ttu-id="2ec67-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="2ec67-643">Opciones</span><span class="sxs-lookup"><span data-stu-id="2ec67-643">Options</span></span>

<span data-ttu-id="2ec67-644"><xref:Microsoft.Extensions.Hosting.HostOptions> configura opciones para <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-644"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="2ec67-645">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="2ec67-645">Shutdown timeout</span></span>

<span data-ttu-id="2ec67-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="2ec67-647">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-647">The default value is five seconds.</span></span>

<span data-ttu-id="2ec67-648">La siguiente configuración de opción en `Program.Main` aumenta el tiempo de espera de apagado predeterminado de 5 segundos a 20:</span><span class="sxs-lookup"><span data-stu-id="2ec67-648">The following option configuration in `Program.Main` increases the default five-second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="2ec67-649">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="2ec67-649">Default services</span></span>

<span data-ttu-id="2ec67-650">Estos son los servicios que se registran durante la inicialización del host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-650">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="2ec67-651">[Entorno](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="2ec67-651">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="2ec67-652">[Configuración](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="2ec67-652">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="2ec67-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="2ec67-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="2ec67-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="2ec67-655">[Opciones](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="2ec67-655">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="2ec67-656">[Registro](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="2ec67-656">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="2ec67-657">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="2ec67-657">Host configuration</span></span>

<span data-ttu-id="2ec67-658">La configuración del host se crea:</span><span class="sxs-lookup"><span data-stu-id="2ec67-658">Host configuration is created by:</span></span>

* <span data-ttu-id="2ec67-659">Llamando a métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder> para establecer la [raíz del contenido](#content-root) y el [entorno](#environment).</span><span class="sxs-lookup"><span data-stu-id="2ec67-659">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="2ec67-660">Leyendo la configuración desde los proveedores de configuración de <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-660">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="2ec67-661">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="2ec67-661">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="2ec67-662">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="2ec67-662">Application key (name)</span></span>

<span data-ttu-id="2ec67-663">La propiedad [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) se establece desde la configuración del host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="2ec67-663">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="2ec67-664">Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="2ec67-664">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="2ec67-665">**Clave**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="2ec67-665">**Key**: `applicationName`</span></span>  
<span data-ttu-id="2ec67-666">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-666">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-667">**Predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-667">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="2ec67-668">**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="2ec67-668">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="2ec67-669">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="2ec67-669">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="2ec67-670">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="2ec67-670">Content root</span></span>

<span data-ttu-id="2ec67-671">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="2ec67-671">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="2ec67-672">**Clave**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-672">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="2ec67-673">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-673">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-674">**Predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-674">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="2ec67-675">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="2ec67-675">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="2ec67-676">**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="2ec67-676">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="2ec67-677">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-677">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="2ec67-678">Para más información, consulte [Aspectos básicos: Raíz del contenido](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="2ec67-678">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="2ec67-679">Entorno</span><span class="sxs-lookup"><span data-stu-id="2ec67-679">Environment</span></span>

<span data-ttu-id="2ec67-680">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-680">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="2ec67-681">**Clave**: `environment`</span><span class="sxs-lookup"><span data-stu-id="2ec67-681">**Key**: `environment`</span></span>  
<span data-ttu-id="2ec67-682">**Tipo**: `string`</span><span class="sxs-lookup"><span data-stu-id="2ec67-682">**Type**: `string`</span></span>  
<span data-ttu-id="2ec67-683">**Predeterminado**: `Production`</span><span class="sxs-lookup"><span data-stu-id="2ec67-683">**Default**: `Production`</span></span>  
<span data-ttu-id="2ec67-684">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="2ec67-684">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="2ec67-685">**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="2ec67-685">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="2ec67-686">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-686">The environment can be set to any value.</span></span> <span data-ttu-id="2ec67-687">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-687">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2ec67-688">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="2ec67-688">Values aren't case-sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="2ec67-689">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="2ec67-689">ConfigureHostConfiguration</span></span>

<span data-ttu-id="2ec67-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para el host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="2ec67-691">La configuración del host se usa para inicializar el <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-691">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="2ec67-692">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-693">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-693">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="2ec67-694">De forma predeterminada no se incluye ningún proveedor.</span><span class="sxs-lookup"><span data-stu-id="2ec67-694">No providers are included by default.</span></span> <span data-ttu-id="2ec67-695">Debe especificar de forma explícita los proveedores de configuración que requiere la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, así como:</span><span class="sxs-lookup"><span data-stu-id="2ec67-695">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="2ec67-696">La configuración de archivo (por ejemplo, de un archivo *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="2ec67-696">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="2ec67-697">La configuración de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-697">Environment variable configuration.</span></span>
* <span data-ttu-id="2ec67-698">La configuración del argumento de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-698">Command-line argument configuration.</span></span>
* <span data-ttu-id="2ec67-699">Otros proveedores de configuración necesarios.</span><span class="sxs-lookup"><span data-stu-id="2ec67-699">Any other required configuration providers.</span></span>

<span data-ttu-id="2ec67-700">La configuración de archivo del host se habilita especificando la ruta base de la aplicación con `SetBasePath` seguido de una llamada a uno de los [proveedores de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2ec67-700">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="2ec67-701">La aplicación de ejemplo usa un archivo JSON, *hostsettings.json*, y llama a <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> para consumir los valores de configuración del host del archivo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-701">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="2ec67-702">Para agregar una [configuración de la variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) del host, llame a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el generador del host.</span><span class="sxs-lookup"><span data-stu-id="2ec67-702">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="2ec67-703">`AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="2ec67-703">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="2ec67-704">La aplicación de ejemplo usa el prefijo `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-704">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="2ec67-705">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ec67-705">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="2ec67-706">Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="2ec67-706">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="2ec67-707">Durante el desarrollo, al usar [Visual Studio](https://visualstudio.microsoft.com) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2ec67-707">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="2ec67-708">En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-708">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="2ec67-709">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-709">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="2ec67-710">La [configuración de la línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) se agrega llamando a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-710">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="2ec67-711">La configuración de la línea de comandos se agrega en último lugar para permitir que los argumentos de la línea de comandos invaliden la configuración proporcionada por los proveedores de configuración anteriores.</span><span class="sxs-lookup"><span data-stu-id="2ec67-711">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="2ec67-712">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2ec67-712">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="2ec67-713">Se puede proporcionar una configuración adicional con las claves [applicationName](#application-key-name) y [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="2ec67-713">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="2ec67-714">Configuración de `HostBuilder` de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="2ec67-714">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="2ec67-715">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="2ec67-715">ConfigureAppConfiguration</span></span>

<span data-ttu-id="2ec67-716">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en la implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-716">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="2ec67-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="2ec67-718">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="2ec67-719">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-719">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="2ec67-720">La configuración creada por <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y en <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-720">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="2ec67-721">La configuración de la aplicación recibe automáticamente la configuración del host proporcionada por [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="2ec67-721">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="2ec67-722">Configuración de aplicación de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="2ec67-722">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="2ec67-723">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2ec67-723">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="2ec67-724">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="2ec67-724">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="2ec67-725">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="2ec67-725">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="2ec67-726">Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ec67-726">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="2ec67-727">La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento `<Content>` siguiente:</span><span class="sxs-lookup"><span data-stu-id="2ec67-727">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="2ec67-728">Los métodos de extensión de configuración, como <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> y <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> requieren paquetes NuGet adicionales, como [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) y [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="2ec67-728">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="2ec67-729">A menos que la aplicación use el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), además del paquete principal [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration), se deben agregar estos paquetes.</span><span class="sxs-lookup"><span data-stu-id="2ec67-729">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="2ec67-730">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-730">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="2ec67-731">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="2ec67-731">ConfigureServices</span></span>

<span data-ttu-id="2ec67-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2ec67-733">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="2ec67-734">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-734">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2ec67-735">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-735">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="2ec67-736">La [aplicación de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ec67-736">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="2ec67-737">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="2ec67-737">ConfigureLogging</span></span>

<span data-ttu-id="2ec67-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> agrega un delegado para configurar el <xref:Microsoft.Extensions.Logging.ILoggingBuilder> proporcionado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="2ec67-739">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="2ec67-740">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-740">UseConsoleLifetime</span></span>

<span data-ttu-id="2ec67-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> escucha <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM, y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="2ec67-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="2ec67-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="2ec67-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="2ec67-744">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="2ec67-744">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="2ec67-745">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="2ec67-745">Container configuration</span></span>

<span data-ttu-id="2ec67-746">Para permitir la conexión a otros contenedores, el host puede aceptar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-746">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="2ec67-747">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-747">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="2ec67-748">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-748">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="2ec67-749">La configuración personalizada del contenedor está administrada por el método <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-749">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="2ec67-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="2ec67-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="2ec67-751">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="2ec67-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="2ec67-752">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ec67-752">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="2ec67-753">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="2ec67-753">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="2ec67-754">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ec67-754">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="2ec67-755">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="2ec67-755">Extensibility</span></span>

<span data-ttu-id="2ec67-756">La extensibilidad de host se realiza con métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-756">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="2ec67-757">En el ejemplo siguiente se muestra cómo un método de extensión extiende una implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder> con el ejemplo [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) demostrado en <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-757">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="2ec67-758">Una aplicación establece el método de extensión `UseHostedService` para registrar el servicio hospedado pasado en `T`:</span><span class="sxs-lookup"><span data-stu-id="2ec67-758">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="2ec67-759">Administración del host</span><span class="sxs-lookup"><span data-stu-id="2ec67-759">Manage the host</span></span>

<span data-ttu-id="2ec67-760">La implementación de <xref:Microsoft.Extensions.Hosting.IHost> es la responsable de iniciar y detener las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="2ec67-760">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="2ec67-761">Run</span><span class="sxs-lookup"><span data-stu-id="2ec67-761">Run</span></span>

<span data-ttu-id="2ec67-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="2ec67-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="2ec67-763">RunAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-763">RunAsync</span></span>

<span data-ttu-id="2ec67-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="2ec67-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="2ec67-765">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-765">RunConsoleAsync</span></span>

<span data-ttu-id="2ec67-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host, y espera a <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM para el apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="2ec67-767">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-767">Start and StopAsync</span></span>

<span data-ttu-id="2ec67-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="2ec67-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="2ec67-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="2ec67-770">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-770">StartAsync and StopAsync</span></span>

<span data-ttu-id="2ec67-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="2ec67-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="2ec67-773">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="2ec67-773">WaitForShutdown</span></span>

<span data-ttu-id="2ec67-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> se desencadena mediante <xref:Microsoft.Extensions.Hosting.IHostLifetime>, como `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (escucha <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="2ec67-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM).</span></span> <span data-ttu-id="2ec67-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="2ec67-776">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="2ec67-776">WaitForShutdownAsync</span></span>

<span data-ttu-id="2ec67-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="2ec67-778">Control externo</span><span class="sxs-lookup"><span data-stu-id="2ec67-778">External control</span></span>

<span data-ttu-id="2ec67-779">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="2ec67-779">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="2ec67-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="2ec67-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="2ec67-781">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="2ec67-781">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="2ec67-782">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="2ec67-782">IHostingEnvironment interface</span></span>

<span data-ttu-id="2ec67-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="2ec67-784">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="2ec67-784">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="2ec67-785">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="2ec67-785">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="2ec67-786">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2ec67-786">IApplicationLifetime interface</span></span>

<span data-ttu-id="2ec67-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="2ec67-788">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos <xref:System.Action> que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="2ec67-788">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="2ec67-789">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="2ec67-789">Cancellation Token</span></span> | <span data-ttu-id="2ec67-790">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="2ec67-790">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="2ec67-791">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="2ec67-791">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="2ec67-792">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-792">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="2ec67-793">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2ec67-793">All requests should be processed.</span></span> <span data-ttu-id="2ec67-794">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="2ec67-794">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="2ec67-795">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="2ec67-795">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="2ec67-796">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2ec67-796">Requests may still be processing.</span></span> <span data-ttu-id="2ec67-797">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="2ec67-797">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="2ec67-798">Inserción de constructor del servicio <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="2ec67-798">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="2ec67-799">En la [aplicación de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de <xref:Microsoft.Extensions.Hosting.IHostedService>) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="2ec67-799">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="2ec67-800">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ec67-800">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="2ec67-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ec67-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="2ec67-802">La siguiente clase usa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="2ec67-802">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="2ec67-803">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2ec67-803">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
