---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico en .NET, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9943c9dd2d6dd67a79186ee880b181a5915d06be
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505718"
---
# <a name="net-generic-host"></a><span data-ttu-id="96ad9-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="96ad9-103">.NET Generic Host</span></span>

<span data-ttu-id="96ad9-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96ad9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="96ad9-105">Las aplicaciones de .NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="96ad9-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="96ad9-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="96ad9-107">En este tema se trata el host genérico de ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), muy útil para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="96ad9-107">This topic covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="96ad9-108">Para la cobertura del host de web (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), consulte <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-108">For coverage of the Web Host (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="96ad9-109">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="96ad9-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="96ad9-110">Se trata de la mensajería, tareas en segundo plano y otras cargas de trabajo no HTTP basadas en la ventaja de host genérico de capacidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="96ad9-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="96ad9-111">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="96ad9-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="96ad9-112">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="96ad9-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="96ad9-113">El host genérico se está desarrollando para reemplazar el host web en una próxima versión y actuar como la API del host principal tanto en escenarios que sean del tipo HTTP como los que no.</span><span class="sxs-lookup"><span data-stu-id="96ad9-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="96ad9-114">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96ad9-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="96ad9-115">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="96ad9-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="96ad9-116">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="96ad9-117">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="96ad9-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="96ad9-118">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="96ad9-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="96ad9-119">En la configuración de **.NET Core Launch (console)**, busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="96ad9-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="96ad9-120">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="96ad9-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="96ad9-121">Introduction</span></span>

<span data-ttu-id="96ad9-122">La biblioteca de host genérico está disponible en el espacio de nombres <xref:Microsoft.Extensions.Hosting> y la proporciona el paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="96ad9-122">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="96ad9-123">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="96ad9-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="96ad9-124"><xref:Microsoft.Extensions.Hosting.IHostedService> es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="96ad9-124"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="96ad9-125">Cada implementación de `IHostedService` se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="96ad9-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="96ad9-126">Se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> en cada `IHostedService` cuando se inicia el host, y se llama a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="96ad9-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="96ad9-127">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="96ad9-127">Set up a host</span></span>

<span data-ttu-id="96ad9-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="96ad9-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="96ad9-129">Servicios predeterminados</span><span class="sxs-lookup"><span data-stu-id="96ad9-129">Default services</span></span>

<span data-ttu-id="96ad9-130">Estos son los servicios que se registran durante la inicialización del host:</span><span class="sxs-lookup"><span data-stu-id="96ad9-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="96ad9-131">[Entorno](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="96ad9-132">[Configuración](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="96ad9-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="96ad9-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="96ad9-135">[Opciones](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="96ad9-136">[Registro](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="96ad9-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="96ad9-137">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="96ad9-137">Host configuration</span></span>

<span data-ttu-id="96ad9-138">La configuración del host se crea:</span><span class="sxs-lookup"><span data-stu-id="96ad9-138">Host configuration is created by:</span></span>

* <span data-ttu-id="96ad9-139">Llamando a métodos de extensión en <xref:Microsoft.Extensions.Hosting.IHostBuilder> para establecer la [raíz del contenido](#content-root) y el [entorno](#environment).</span><span class="sxs-lookup"><span data-stu-id="96ad9-139">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="96ad9-140">Leyendo la configuración desde los proveedores de configuración de <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-140">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="96ad9-141">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="96ad9-141">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="96ad9-142">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="96ad9-142">Application key (name)</span></span>

<span data-ttu-id="96ad9-143">La propiedad [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) se establece desde la configuración del host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="96ad9-143">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="96ad9-144">Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="96ad9-144">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="96ad9-145">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="96ad9-145">**Key**: applicationName</span></span>  
<span data-ttu-id="96ad9-146">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96ad9-146">**Type**: *string*</span></span>  
<span data-ttu-id="96ad9-147">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-147">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="96ad9-148">**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="96ad9-148">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="96ad9-149">**Variable de entorno**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96ad9-149">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

### <a name="content-root"></a><span data-ttu-id="96ad9-150">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="96ad9-150">Content root</span></span>

<span data-ttu-id="96ad9-151">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="96ad9-151">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="96ad9-152">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="96ad9-152">**Key**: contentRoot</span></span>  
<span data-ttu-id="96ad9-153">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96ad9-153">**Type**: *string*</span></span>  
<span data-ttu-id="96ad9-154">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-154">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="96ad9-155">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="96ad9-155">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="96ad9-156">**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96ad9-156">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="96ad9-157">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="96ad9-157">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="96ad9-158">Entorno</span><span class="sxs-lookup"><span data-stu-id="96ad9-158">Environment</span></span>

<span data-ttu-id="96ad9-159">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-159">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="96ad9-160">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="96ad9-160">**Key**: environment</span></span>  
<span data-ttu-id="96ad9-161">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96ad9-161">**Type**: *string*</span></span>  
<span data-ttu-id="96ad9-162">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="96ad9-162">**Default**: Production</span></span>  
<span data-ttu-id="96ad9-163">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="96ad9-163">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="96ad9-164">**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96ad9-164">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="96ad9-165">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="96ad9-165">The environment can be set to any value.</span></span> <span data-ttu-id="96ad9-166">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-166">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="96ad9-167">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="96ad9-167">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="96ad9-168">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="96ad9-168">ConfigureHostConfiguration</span></span>

<span data-ttu-id="96ad9-169">`ConfigureHostConfiguration` usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para el host.</span><span class="sxs-lookup"><span data-stu-id="96ad9-169">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="96ad9-170">La configuración del host se usa para inicializar el <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-170">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="96ad9-171">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96ad9-171">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="96ad9-172">El host usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="96ad9-172">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="96ad9-173">La configuración del host fluye automáticamente a la configuración de la aplicación ([ConfigureAppConfiguration](#configureappconfiguration) y al resto de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="96ad9-173">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="96ad9-174">De forma predeterminada no se incluye ningún proveedor.</span><span class="sxs-lookup"><span data-stu-id="96ad9-174">No providers are included by default.</span></span> <span data-ttu-id="96ad9-175">Debe especificar de forma explícita los proveedores de configuración que requiere la aplicación en `ConfigureHostConfiguration`, así como:</span><span class="sxs-lookup"><span data-stu-id="96ad9-175">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="96ad9-176">La configuración de archivo (por ejemplo, de un archivo *hostsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="96ad9-176">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="96ad9-177">La configuración de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="96ad9-177">Environment variable configuration.</span></span>
* <span data-ttu-id="96ad9-178">La configuración del argumento de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="96ad9-178">Command-line argument configuration.</span></span>
* <span data-ttu-id="96ad9-179">Otros proveedores de configuración necesarios.</span><span class="sxs-lookup"><span data-stu-id="96ad9-179">Any other required configuration providers.</span></span>

<span data-ttu-id="96ad9-180">La configuración de archivo del host se habilita especificando la ruta base de la aplicación con `SetBasePath` seguido de una llamada a uno de los [proveedores de configuración de archivo](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="96ad9-180">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="96ad9-181">La aplicación de ejemplo usa un archivo JSON, *hostsettings.json*, y llama a <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> para consumir los valores de configuración del host del archivo.</span><span class="sxs-lookup"><span data-stu-id="96ad9-181">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="96ad9-182">Para agregar una [configuración de la variable de entorno](xref:fundamentals/configuration/index#environment-variables-configuration-provider) del host, llame a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el generador del host.</span><span class="sxs-lookup"><span data-stu-id="96ad9-182">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="96ad9-183">`AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="96ad9-183">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="96ad9-184">La aplicación de ejemplo usa el prefijo `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-184">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="96ad9-185">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="96ad9-185">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="96ad9-186">Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-186">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="96ad9-187">Durante el desarrollo, al usar [Visual Studio](https://www.visualstudio.com/) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="96ad9-187">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="96ad9-188">En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="96ad9-188">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="96ad9-189">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-189">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="96ad9-190">La [configuración de la línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) se agrega llamando a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-190">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="96ad9-191">La configuración de la línea de comandos se agrega en último lugar para permitir que los argumentos de la línea de comandos invaliden la configuración proporcionada por los proveedores de configuración anteriores.</span><span class="sxs-lookup"><span data-stu-id="96ad9-191">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="96ad9-192">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96ad9-192">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="96ad9-193">Se puede proporcionar una configuración adicional con las claves [applicationName](#application-key-name) y [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="96ad9-193">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="96ad9-194">Configuración de `HostBuilder` de ejemplo con `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="96ad9-194">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="96ad9-195">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="96ad9-195">ConfigureAppConfiguration</span></span>

<span data-ttu-id="96ad9-196">La configuración de la aplicación se crea llamando a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> en la implementación de <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-196">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="96ad9-197">`ConfigureAppConfiguration` usa un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> para crear una <xref:Microsoft.Extensions.Configuration.IConfiguration> para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-197">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="96ad9-198">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96ad9-198">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="96ad9-199">La aplicación usa cualquier opción que establezca un valor en último lugar en una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="96ad9-199">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="96ad9-200">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) para las operaciones posteriores y en <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-200">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="96ad9-201">La configuración de la aplicación recibe automáticamente la configuración del host proporcionada por [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="96ad9-201">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="96ad9-202">Configuración de aplicación de ejemplo con `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="96ad9-202">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="96ad9-203">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96ad9-203">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="96ad9-204">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="96ad9-204">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="96ad9-205">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="96ad9-205">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="96ad9-206">Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="96ad9-206">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="96ad9-207">La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento `<Content>` siguiente:</span><span class="sxs-lookup"><span data-stu-id="96ad9-207">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="96ad9-208">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="96ad9-208">ConfigureServices</span></span>

<span data-ttu-id="96ad9-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-209"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="96ad9-210">Se puede llamar varias veces a `ConfigureServices` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96ad9-210">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="96ad9-211">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-211">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="96ad9-212">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-212">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="96ad9-213">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96ad9-213">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="96ad9-214">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="96ad9-214">ConfigureLogging</span></span>

<span data-ttu-id="96ad9-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> agrega un delegado para configurar el <xref:Microsoft.Extensions.Logging.ILoggingBuilder> proporcionado.</span><span class="sxs-lookup"><span data-stu-id="96ad9-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="96ad9-216">Se puede llamar varias veces a `ConfigureLogging` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96ad9-216">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="96ad9-217">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="96ad9-217">UseConsoleLifetime</span></span>

<span data-ttu-id="96ad9-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> escucha `Ctrl+C`/SIGINT o SIGTERM y llama a <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="96ad9-218"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="96ad9-219">`UseConsoleLifetime` desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="96ad9-219">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="96ad9-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="96ad9-220"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="96ad9-221">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="96ad9-221">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="96ad9-222">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="96ad9-222">Container configuration</span></span>

<span data-ttu-id="96ad9-223">Para permitir la conexión a otros contenedores, el host puede aceptar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-223">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="96ad9-224">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="96ad9-224">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="96ad9-225">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-225">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="96ad9-226">La configuración personalizada del contenedor está administrada por el método <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-226">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="96ad9-227">`ConfigureContainer` proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="96ad9-227">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="96ad9-228">Se puede llamar varias veces a `ConfigureContainer` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96ad9-228">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="96ad9-229">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96ad9-229">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="96ad9-230">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="96ad9-230">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="96ad9-231">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96ad9-231">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="96ad9-232">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="96ad9-232">Extensibility</span></span>

<span data-ttu-id="96ad9-233">La extensibilidad de host se realiza con métodos de extensión en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="96ad9-233">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="96ad9-234">En el ejemplo siguiente se muestra cómo un método de extensión extiende una implementación de `IHostBuilder` con el ejemplo [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) demostrado en <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-234">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="96ad9-235">Una aplicación establece el método de extensión `UseHostedService` para registrar el servicio hospedado pasado en `T`:</span><span class="sxs-lookup"><span data-stu-id="96ad9-235">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="96ad9-236">Administración del host</span><span class="sxs-lookup"><span data-stu-id="96ad9-236">Manage the host</span></span>

<span data-ttu-id="96ad9-237">La implementación de <xref:Microsoft.Extensions.Hosting.IHost> es la responsable de iniciar y detener las implementaciones de `IHostedService` que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="96ad9-237">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="96ad9-238">Run</span><span class="sxs-lookup"><span data-stu-id="96ad9-238">Run</span></span>

<span data-ttu-id="96ad9-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="96ad9-239"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="96ad9-240">RunAsync</span><span class="sxs-lookup"><span data-stu-id="96ad9-240">RunAsync</span></span>

<span data-ttu-id="96ad9-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> inicia la aplicación y devuelve `Task`, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="96ad9-241"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="96ad9-242">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="96ad9-242">RunConsoleAsync</span></span>

<span data-ttu-id="96ad9-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre `Ctrl+C`/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="96ad9-243"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="96ad9-244">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="96ad9-244">Start and StopAsync</span></span>

<span data-ttu-id="96ad9-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="96ad9-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="96ad9-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="96ad9-246"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="96ad9-247">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="96ad9-247">StartAsync and StopAsync</span></span>

<span data-ttu-id="96ad9-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-248"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="96ad9-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-249"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="96ad9-250">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="96ad9-250">WaitForShutdown</span></span>

<span data-ttu-id="96ad9-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> se desencadena mediante <xref:Microsoft.Extensions.Hosting.IHostLifetime>, como, por ejemplo, <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (escucha `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="96ad9-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="96ad9-252">`WaitForShutdown` llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-252">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="96ad9-253">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="96ad9-253">WaitForShutdownAsync</span></span>

<span data-ttu-id="96ad9-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> devuelve `Task`, que se completa cuando se desencadena el cierre a través del token determinado y llama a <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="96ad9-255">Control externo</span><span class="sxs-lookup"><span data-stu-id="96ad9-255">External control</span></span>

<span data-ttu-id="96ad9-256">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="96ad9-256">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="96ad9-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> se llama al inicio de <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="96ad9-257"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="96ad9-258">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="96ad9-258">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="96ad9-259">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="96ad9-259">IHostingEnvironment interface</span></span>

<span data-ttu-id="96ad9-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-260"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="96ad9-261">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="96ad9-261">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="96ad9-262">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="96ad9-262">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="96ad9-263">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="96ad9-263">IApplicationLifetime interface</span></span>

<span data-ttu-id="96ad9-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="96ad9-264"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="96ad9-265">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="96ad9-265">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="96ad9-266">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="96ad9-266">Cancellation Token</span></span> | <span data-ttu-id="96ad9-267">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="96ad9-267">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="96ad9-268">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="96ad9-268">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="96ad9-269">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="96ad9-269">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="96ad9-270">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="96ad9-270">All requests should be processed.</span></span> <span data-ttu-id="96ad9-271">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="96ad9-271">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="96ad9-272">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="96ad9-272">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="96ad9-273">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="96ad9-273">Requests may still be processing.</span></span> <span data-ttu-id="96ad9-274">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="96ad9-274">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="96ad9-275">Inserción de constructor del servicio `IApplicationLifetime` en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="96ad9-275">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="96ad9-276">En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de `IHostedService`) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="96ad9-276">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="96ad9-277">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="96ad9-277">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="96ad9-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96ad9-278"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="96ad9-279">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="96ad9-279">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="96ad9-280">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96ad9-280">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="96ad9-281">Ejemplos de hospedaje de repositorios en GitHub</span><span class="sxs-lookup"><span data-stu-id="96ad9-281">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
