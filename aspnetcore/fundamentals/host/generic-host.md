---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico de ASP.NET Core, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: bb6afe59fcad685d18cdc9c8d90cfcc7b3a6541d
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "58809200"
---
# <a name="net-generic-host"></a><span data-ttu-id="3d71d-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="3d71d-103">.NET Generic Host</span></span>

<span data-ttu-id="3d71d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3d71d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3d71d-105">Las aplicaciones ASP.NET Core configuran e inician un host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-105">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="3d71d-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-106">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="3d71d-107">En este artículo se describe el host genérico de .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span><span class="sxs-lookup"><span data-stu-id="3d71d-107">This article covers the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

<span data-ttu-id="3d71d-108">El host genérico se diferencia del host web en que desacopla la canalización HTTP de la API de host web para habilitar una mayor variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-108">Generic Host differs from Web Host in that it decouples the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="3d71d-109">La mensajería, las tareas en segundo plano y otras cargas de trabajo que no son de HTTP pueden usar el host genérico y beneficiarse de funcionalidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="3d71d-109">Messaging, background tasks, and other non-HTTP workloads can use Generic Host and benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="3d71d-110">A partir de ASP.NET Core 3.0, se recomienda el host genérico para las cargas de trabajo de HTTP y las que no sean de HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d71d-110">Starting in ASP.NET Core 3.0, Generic Host is recommended for both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="3d71d-111">Una implementación de servidor HTTP, si se incluye, se ejecuta como una implementación de <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-111">An HTTP server implementation, if included, runs as an implementation of <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="3d71d-112"><xref:Microsoft.Extensions.Hosting.IHostedService> es una interfaz que también se puede usar para otras cargas de trabajo.</span><span class="sxs-lookup"><span data-stu-id="3d71d-112"><xref:Microsoft.Extensions.Hosting.IHostedService> is an interface that can be used for other workloads as well.</span></span>

<span data-ttu-id="3d71d-113">El host web ya no se recomienda para las aplicaciones web, pero sigue estando disponible para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="3d71d-113">Web Host is no longer recommended for web apps but remains available for backward compatibility.</span></span>

> [!NOTE]
> <span data-ttu-id="3d71d-114">El resto de este artículo no se ha actualizado para la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="3d71d-114">This remainder of this article hasn't been updated for 3.0.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3d71d-115">Las aplicaciones ASP.NET Core configuran e inician un host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-115">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="3d71d-116">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-116">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="3d71d-117">En este artículo se trata el host genérico de ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), que se usa para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d71d-117">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="3d71d-118">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una mayor variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-118">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="3d71d-119">La mensajería, las tareas en segundo plano y otras cargas de trabajo que no son de HTTP basadas en la ventaja de host genérico se benefician de funcionalidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="3d71d-119">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="3d71d-120">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="3d71d-120">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="3d71d-121">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="3d71d-121">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="3d71d-122">El host genérico reemplazará al host web en una próxima versión y actuará como la API de host principal tanto en escenarios de HTTP como los que no lo son.</span><span class="sxs-lookup"><span data-stu-id="3d71d-122">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

::: moniker-end

<span data-ttu-id="3d71d-123">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3d71d-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3d71d-124">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="3d71d-124">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="3d71d-125">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="3d71d-125">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="3d71d-126">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="3d71d-126">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="3d71d-127">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="3d71d-127">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="3d71d-128">En la configuración de **.NET Core Launch (console)**, busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="3d71d-128">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="3d71d-129">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="3d71d-129">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="3d71d-130">Introducción</span><span class="sxs-lookup"><span data-stu-id="3d71d-130">Introduction</span></span>

<span data-ttu-id="3d71d-131">La biblioteca de host genérico está disponible en el espacio de nombres <xref:Microsoft.Extensions.Hosting> y la proporciona el paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="3d71d-131">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="3d71d-132">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="3d71d-132">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="3d71d-133"><xref:Microsoft.Extensions.Hosting.IHostedService> es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="3d71d-133"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="3d71d-134">Cada implementación de <xref:Microsoft.Extensions.Hosting.IHostedService> se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="3d71d-134">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="3d71d-135">Se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> en cada <xref:Microsoft.Extensions.Hosting.IHostedService> cuando se inicia el host, y se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="3d71d-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3d71d-136">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="3d71d-136">Set up a host</span></span>

<span data-ttu-id="3d71d-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="3d71d-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="3d71d-138">Opciones</span><span class="sxs-lookup"><span data-stu-id="3d71d-138">Options</span></span>

<span data-ttu-id="3d71d-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configura opciones para <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="3d71d-140">Tiempo de espera de apagado</span><span class="sxs-lookup"><span data-stu-id="3d71d-140">Shutdown timeout</span></span>

<span data-ttu-id="3d71d-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> establece el tiempo de espera para <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="3d71d-142">El valor predeterminado es cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="3d71d-142">The default value is five seconds.</span></span>

<span data-ttu-id="3d71d-143">La siguiente configuración de opción en `Program.Main` aumenta el tiempo de espera de apagado predeterminado de 5 segundos a 20 segundos:</span><span class="sxs-lookup"><span data-stu-id="3d71d-143">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="3d71d-144">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="3d71d-144">Default services</span></span>

<span data-ttu-id="3d71d-145">Estos son los servicios que se registran durante la inicialización del host:</span><span class="sxs-lookup"><span data-stu-id="3d71d-145">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="3d71d-146">[Entorno](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-146">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="3d71d-147">[Configuración](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-147">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="3d71d-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="3d71d-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="3d71d-150">[Opciones](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-150">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="3d71d-151">[Registro](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="3d71d-151">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="3d71d-152">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="3d71d-152">Host configuration</span></span>

<span data-ttu-id="3d71d-153">La configuración del host se crea:</span><span class="sxs-lookup"><span data-stu-id="3d71d-153">Host configuration is created by:</span></span>

* <span data-ttu-id="3d71d-154">Llamando a métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder> para establecer la [raíz del contenido](#content-root) y el [entorno](#environment).</span><span class="sxs-lookup"><span data-stu-id="3d71d-154">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="3d71d-155">Leyendo la configuración desde los proveedores de configuración de <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-155">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="3d71d-156">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="3d71d-156">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="3d71d-157">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="3d71d-157">Application key (name)</span></span>

<span data-ttu-id="3d71d-158">La propiedad [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) se establece desde la configuración del host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="3d71d-158">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="3d71d-159">Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="3d71d-159">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="3d71d-160">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="3d71d-160">**Key**: applicationName</span></span>  
<span data-ttu-id="3d71d-161">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="3d71d-161">**Type**: *string*</span></span>  
<span data-ttu-id="3d71d-162">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-162">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="3d71d-163">**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="3d71d-163">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="3d71d-164">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="3d71d-164">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="3d71d-165">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="3d71d-165">Content root</span></span>

<span data-ttu-id="3d71d-166">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="3d71d-166">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="3d71d-167">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3d71d-167">**Key**: contentRoot</span></span>  
<span data-ttu-id="3d71d-168">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="3d71d-168">**Type**: *string*</span></span>  
<span data-ttu-id="3d71d-169">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-169">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3d71d-170">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3d71d-170">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3d71d-171">**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="3d71d-171">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="3d71d-172">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="3d71d-172">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="3d71d-173">Entorno</span><span class="sxs-lookup"><span data-stu-id="3d71d-173">Environment</span></span>

<span data-ttu-id="3d71d-174">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-174">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3d71d-175">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="3d71d-175">**Key**: environment</span></span>  
<span data-ttu-id="3d71d-176">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="3d71d-176">**Type**: *string*</span></span>  
<span data-ttu-id="3d71d-177">**Valor predeterminado**: Producción</span><span class="sxs-lookup"><span data-stu-id="3d71d-177">**Default**: Production</span></span>  
<span data-ttu-id="3d71d-178">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3d71d-178">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3d71d-179">**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="3d71d-179">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="3d71d-180">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="3d71d-180">The environment can be set to any value.</span></span> <span data-ttu-id="3d71d-181">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="3d71d-181">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3d71d-182">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3d71d-182">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="3d71d-183">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="3d71d-183">ConfigureHostConfiguration</span></span>

<span data-ttu-id="3d71d-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para el host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="3d71d-185">La configuración del host se usa para inicializar el <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-185">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="3d71d-186">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3d71d-186"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="3d71d-187">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="3d71d-187">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="3d71d-188">La configuración del host fluye automáticamente a la configuración de la aplicación ([ConfigureAppConfiguration](#configureappconfiguration) y al resto de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="3d71d-188">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="3d71d-189">De forma predeterminada no se incluye ningún proveedor.</span><span class="sxs-lookup"><span data-stu-id="3d71d-189">No providers are included by default.</span></span> <span data-ttu-id="3d71d-190">Debe especificar de forma explícita los proveedores de configuración que requiere la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, así como:</span><span class="sxs-lookup"><span data-stu-id="3d71d-190">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="3d71d-191">La configuración de archivo (por ejemplo, de un archivo *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="3d71d-191">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="3d71d-192">La configuración de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="3d71d-192">Environment variable configuration.</span></span>
* <span data-ttu-id="3d71d-193">La configuración del argumento de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="3d71d-193">Command-line argument configuration.</span></span>
* <span data-ttu-id="3d71d-194">Otros proveedores de configuración necesarios.</span><span class="sxs-lookup"><span data-stu-id="3d71d-194">Any other required configuration providers.</span></span>

<span data-ttu-id="3d71d-195">La configuración de archivo del host se habilita especificando la ruta base de la aplicación con `SetBasePath` seguido de una llamada a uno de los [proveedores de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="3d71d-195">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="3d71d-196">La aplicación de ejemplo usa un archivo JSON, *hostsettings.json*, y llama a <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> para consumir los valores de configuración del host del archivo.</span><span class="sxs-lookup"><span data-stu-id="3d71d-196">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="3d71d-197">Para agregar una [configuración de la variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) del host, llame a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el generador del host.</span><span class="sxs-lookup"><span data-stu-id="3d71d-197">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="3d71d-198">`AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="3d71d-198">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="3d71d-199">La aplicación de ejemplo usa el prefijo `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="3d71d-199">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="3d71d-200">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="3d71d-200">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="3d71d-201">Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="3d71d-201">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="3d71d-202">Durante el desarrollo, al usar [Visual Studio](https://www.visualstudio.com/) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3d71d-202">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="3d71d-203">En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="3d71d-203">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="3d71d-204">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-204">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="3d71d-205">La [configuración de la línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) se agrega llamando a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-205">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="3d71d-206">La configuración de la línea de comandos se agrega en último lugar para permitir que los argumentos de la línea de comandos invaliden la configuración proporcionada por los proveedores de configuración anteriores.</span><span class="sxs-lookup"><span data-stu-id="3d71d-206">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="3d71d-207">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3d71d-207">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="3d71d-208">Se puede proporcionar una configuración adicional con las claves [applicationName](#application-key-name) y [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="3d71d-208">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="3d71d-209">Configuración de `HostBuilder` de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="3d71d-209">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="3d71d-210">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="3d71d-210">ConfigureAppConfiguration</span></span>

<span data-ttu-id="3d71d-211">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en la implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-211">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="3d71d-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="3d71d-213">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3d71d-213"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="3d71d-214">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="3d71d-214">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="3d71d-215">La configuración creada por <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y en <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-215">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="3d71d-216">La configuración de la aplicación recibe automáticamente la configuración del host proporcionada por [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="3d71d-216">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="3d71d-217">Configuración de aplicación de ejemplo con <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="3d71d-217">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="3d71d-218">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3d71d-218">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="3d71d-219">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="3d71d-219">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="3d71d-220">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="3d71d-220">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="3d71d-221">Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="3d71d-221">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="3d71d-222">La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento `<Content>` siguiente:</span><span class="sxs-lookup"><span data-stu-id="3d71d-222">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="3d71d-223">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="3d71d-223">ConfigureServices</span></span>

<span data-ttu-id="3d71d-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3d71d-225">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3d71d-225"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="3d71d-226">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-226">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="3d71d-227">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-227">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="3d71d-228">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3d71d-228">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="3d71d-229">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="3d71d-229">ConfigureLogging</span></span>

<span data-ttu-id="3d71d-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> agrega un delegado para configurar el <xref:Microsoft.Extensions.Logging.ILoggingBuilder> proporcionado.</span><span class="sxs-lookup"><span data-stu-id="3d71d-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="3d71d-231">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3d71d-231"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="3d71d-232">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="3d71d-232">UseConsoleLifetime</span></span>

<span data-ttu-id="3d71d-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> escucha Ctrl+C/SIGINT o SIGTERM y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="3d71d-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="3d71d-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="3d71d-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="3d71d-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3d71d-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="3d71d-236">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="3d71d-236">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="3d71d-237">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="3d71d-237">Container configuration</span></span>

<span data-ttu-id="3d71d-238">Para permitir la conexión a otros contenedores, el host puede aceptar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-238">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="3d71d-239">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="3d71d-239">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="3d71d-240">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="3d71d-241">La configuración personalizada del contenedor está administrada por el método <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-241">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="3d71d-242"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="3d71d-242"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="3d71d-243">Se puede llamar varias veces a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3d71d-243"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="3d71d-244">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3d71d-244">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="3d71d-245">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="3d71d-245">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="3d71d-246">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3d71d-246">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="3d71d-247">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="3d71d-247">Extensibility</span></span>

<span data-ttu-id="3d71d-248">La extensibilidad de host se realiza con métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-248">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="3d71d-249">En el ejemplo siguiente se muestra cómo un método de extensión extiende una implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder> con el ejemplo [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) demostrado en <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-249">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="3d71d-250">Una aplicación establece el método de extensión `UseHostedService` para registrar el servicio hospedado pasado en `T`:</span><span class="sxs-lookup"><span data-stu-id="3d71d-250">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="3d71d-251">Administración del host</span><span class="sxs-lookup"><span data-stu-id="3d71d-251">Manage the host</span></span>

<span data-ttu-id="3d71d-252">La implementación de <xref:Microsoft.Extensions.Hosting.IHost> es la responsable de iniciar y detener las implementaciones de <xref:Microsoft.Extensions.Hosting.IHostedService> que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="3d71d-252">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="3d71d-253">Run</span><span class="sxs-lookup"><span data-stu-id="3d71d-253">Run</span></span>

<span data-ttu-id="3d71d-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="3d71d-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="3d71d-255">RunAsync</span><span class="sxs-lookup"><span data-stu-id="3d71d-255">RunAsync</span></span>

<span data-ttu-id="3d71d-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="3d71d-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="3d71d-257">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="3d71d-257">RunConsoleAsync</span></span>

<span data-ttu-id="3d71d-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre Ctrl+C/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="3d71d-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="3d71d-259">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="3d71d-259">Start and StopAsync</span></span>

<span data-ttu-id="3d71d-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="3d71d-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="3d71d-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="3d71d-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="3d71d-262">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="3d71d-262">StartAsync and StopAsync</span></span>

<span data-ttu-id="3d71d-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="3d71d-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="3d71d-265">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="3d71d-265">WaitForShutdown</span></span>

<span data-ttu-id="3d71d-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> se desencadena mediante <xref:Microsoft.Extensions.Hosting.IHostLifetime>, como <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (escucha Ctrl+C/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3d71d-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="3d71d-267"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-267"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="3d71d-268">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="3d71d-268">WaitForShutdownAsync</span></span>

<span data-ttu-id="3d71d-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve <xref:System.Threading.Tasks.Task>, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="3d71d-270">Control externo</span><span class="sxs-lookup"><span data-stu-id="3d71d-270">External control</span></span>

<span data-ttu-id="3d71d-271">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="3d71d-271">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="3d71d-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="3d71d-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="3d71d-273">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="3d71d-273">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3d71d-274">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="3d71d-274">IHostingEnvironment interface</span></span>

<span data-ttu-id="3d71d-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="3d71d-276">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="3d71d-276">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="3d71d-277">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="3d71d-277">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3d71d-278">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="3d71d-278">IApplicationLifetime interface</span></span>

<span data-ttu-id="3d71d-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="3d71d-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="3d71d-280">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos <xref:System.Action> que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="3d71d-280">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3d71d-281">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="3d71d-281">Cancellation Token</span></span> | <span data-ttu-id="3d71d-282">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="3d71d-282">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="3d71d-283">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="3d71d-283">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="3d71d-284">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="3d71d-284">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3d71d-285">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3d71d-285">All requests should be processed.</span></span> <span data-ttu-id="3d71d-286">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="3d71d-286">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="3d71d-287">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="3d71d-287">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3d71d-288">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3d71d-288">Requests may still be processing.</span></span> <span data-ttu-id="3d71d-289">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="3d71d-289">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="3d71d-290">Inserción de constructor del servicio <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="3d71d-290">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="3d71d-291">En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de <xref:Microsoft.Extensions.Hosting.IHostedService>) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="3d71d-291">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="3d71d-292">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d71d-292">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="3d71d-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3d71d-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="3d71d-294">La siguiente clase usa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="3d71d-294">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3d71d-295">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3d71d-295">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
