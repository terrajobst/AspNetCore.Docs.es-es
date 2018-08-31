---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico en .NET, que es responsable de la administración de inicio y duración de la aplicación.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: de9044875c8ebc62c80a129d721e7d37be5d846d
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927814"
---
# <a name="net-generic-host"></a><span data-ttu-id="96fa9-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="96fa9-103">.NET Generic Host</span></span>

<span data-ttu-id="96fa9-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96fa9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="96fa9-105">Las aplicaciones de .NET Core configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="96fa9-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="96fa9-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="96fa9-107">En este tema se trata el host genérico de ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), muy útil para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="96fa9-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="96fa9-108">Para la cobertura del host de web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="96fa9-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="96fa9-109">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="96fa9-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="96fa9-110">Se trata de la mensajería, tareas en segundo plano y otras cargas de trabajo no HTTP basadas en la ventaja de host genérico de capacidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="96fa9-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="96fa9-111">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="96fa9-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="96fa9-112">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="96fa9-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="96fa9-113">El host genérico se está desarrollando para reemplazar el host web en una próxima versión y actuar como la API del host principal tanto en escenarios que sean del tipo HTTP como los que no.</span><span class="sxs-lookup"><span data-stu-id="96fa9-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="96fa9-114">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96fa9-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="96fa9-115">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="96fa9-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="96fa9-116">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="96fa9-117">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="96fa9-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="96fa9-118">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="96fa9-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="96fa9-119">En la configuración de **.NET Core Launch (console)**, busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="96fa9-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="96fa9-120">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="96fa9-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="96fa9-121">Introduction</span></span>

<span data-ttu-id="96fa9-122">La biblioteca de host genérico está disponible en el [espacio de nombres de Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) y la proporciona el [paquete Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="96fa9-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="96fa9-123">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="96fa9-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="96fa9-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="96fa9-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="96fa9-125">Cada implementación de `IHostedService` se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="96fa9-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="96fa9-126">Se llama a [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) en cada `IHostedService` cuando se inicia el host, y se llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="96fa9-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="96fa9-127">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="96fa9-127">Set up a host</span></span>

<span data-ttu-id="96fa9-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="96fa9-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="96fa9-129">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="96fa9-129">Host configuration</span></span>

<span data-ttu-id="96fa9-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="96fa9-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="96fa9-131">Generador de configuración</span><span class="sxs-lookup"><span data-stu-id="96fa9-131">Configuration builder</span></span>
* <span data-ttu-id="96fa9-132">Configuración del método de extensión</span><span class="sxs-lookup"><span data-stu-id="96fa9-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="96fa9-133">Generador de configuración</span><span class="sxs-lookup"><span data-stu-id="96fa9-133">Configuration builder</span></span>

<span data-ttu-id="96fa9-134">La configuración del generador de host se crea mediante una llamada a [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="96fa9-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="96fa9-135">`ConfigureHostConfiguration` usa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para el host.</span><span class="sxs-lookup"><span data-stu-id="96fa9-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="96fa9-136">El generador de configuración inicializa [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="96fa9-137">La configuración de las variables de entorno no se agrega de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="96fa9-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="96fa9-138">Llame a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el generador de host para configurar el host a partir de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="96fa9-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="96fa9-139">`AddEnvironmentVariables` acepta un prefijo opcional definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="96fa9-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="96fa9-140">La aplicación de ejemplo usa el prefijo `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="96fa9-141">El prefijo se quita cuando se leen las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="96fa9-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="96fa9-142">Al configurar el host de la aplicación de ejemplo, el valor de la variable de entorno de `PREFIX_ENVIRONMENT` se convierte en el valor de configuración de host de la clave `environment`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="96fa9-143">Durante el desarrollo, al usar [Visual Studio](https://www.visualstudio.com/) o al ejecutar una aplicación con `dotnet run`, se pueden establecer variables de entorno en el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="96fa9-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="96fa9-144">En [Visual Studio Code](https://code.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *.vscode/launch.json* durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="96fa9-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="96fa9-145">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="96fa9-145">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="96fa9-146">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96fa9-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="96fa9-147">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="96fa9-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="96fa9-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96fa9-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="96fa9-149">Configuración de `HostBuilder` de ejemplo con `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="96fa9-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="96fa9-150">El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="96fa9-151">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="96fa9-152">El método `AddConfiguration` espera que las claves coincidan con las claves `HostBuilder` (por ejemplo, `environment`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="96fa9-153">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="96fa9-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="96fa9-154">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="96fa9-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="96fa9-155">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="96fa9-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="96fa9-156">Configuración del método de extensión</span><span class="sxs-lookup"><span data-stu-id="96fa9-156">Extension method configuration</span></span>

<span data-ttu-id="96fa9-157">Se llama a métodos de extensión en la implementación de `IHostBuilder` para configurar la raíz de contenido y el entorno.</span><span class="sxs-lookup"><span data-stu-id="96fa9-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="96fa9-158">Clave de aplicación (nombre)</span><span class="sxs-lookup"><span data-stu-id="96fa9-158">Application Key (Name)</span></span>

<span data-ttu-id="96fa9-159">La propiedad [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) se establece desde la configuración del host durante la construcción de este.</span><span class="sxs-lookup"><span data-stu-id="96fa9-159">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="96fa9-160">Para establecer el valor explícitamente, use [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="96fa9-160">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="96fa9-161">**Clave**: nombreDeAplicación</span><span class="sxs-lookup"><span data-stu-id="96fa9-161">**Key**: applicationName</span></span>  
<span data-ttu-id="96fa9-162">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96fa9-162">**Type**: *string*</span></span>  
<span data-ttu-id="96fa9-163">**Valor predeterminado**: nombre del ensamblado que contiene el punto de entrada de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-163">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="96fa9-164">**Establecer mediante**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="96fa9-164">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="96fa9-165">**Variable de entorno**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96fa9-165">**Environment variable**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="96fa9-166">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="96fa9-166">Content Root</span></span>

<span data-ttu-id="96fa9-167">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="96fa9-167">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="96fa9-168">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="96fa9-168">**Key**: contentRoot</span></span>  
<span data-ttu-id="96fa9-169">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96fa9-169">**Type**: *string*</span></span>  
<span data-ttu-id="96fa9-170">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-170">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="96fa9-171">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="96fa9-171">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="96fa9-172">**Variable de entorno**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96fa9-172">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="96fa9-173">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="96fa9-173">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="96fa9-174">Entorno</span><span class="sxs-lookup"><span data-stu-id="96fa9-174">Environment</span></span>

<span data-ttu-id="96fa9-175">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-175">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="96fa9-176">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="96fa9-176">**Key**: environment</span></span>  
<span data-ttu-id="96fa9-177">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="96fa9-177">**Type**: *string*</span></span>  
<span data-ttu-id="96fa9-178">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="96fa9-178">**Default**: Production</span></span>  
<span data-ttu-id="96fa9-179">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="96fa9-179">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="96fa9-180">**Variable de entorno**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` es [opcional y definido por el usuario](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="96fa9-180">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="96fa9-181">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="96fa9-181">The environment can be set to any value.</span></span> <span data-ttu-id="96fa9-182">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-182">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="96fa9-183">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="96fa9-183">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="96fa9-184">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="96fa9-184">ConfigureAppConfiguration</span></span>

<span data-ttu-id="96fa9-185">La configuración del generador de la aplicación se crea mediante una llamada a [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="96fa9-185">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="96fa9-186">`ConfigureAppConfiguration` usa un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear un [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-186">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="96fa9-187">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96fa9-187">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="96fa9-188">La aplicación usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="96fa9-188">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="96fa9-189">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para las operaciones posteriores y en [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="96fa9-189">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="96fa9-190">Configuración de aplicación de ejemplo con `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="96fa9-190">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="96fa9-191">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="96fa9-191">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="96fa9-192">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="96fa9-192">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="96fa9-193">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="96fa9-193">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="96fa9-194">El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-194">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="96fa9-195">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-195">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="96fa9-196">El método `AddConfiguration` espera una coincidencia exacta con las claves de configuración (por ejemplo, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="96fa9-196">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="96fa9-197">La presencia del nombre de sección en las claves evita que los valores de la sección configuren la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-197">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="96fa9-198">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="96fa9-198">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="96fa9-199">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="96fa9-199">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="96fa9-200">Para mover los archivos de configuración al directorio de salida, especifique los archivos de configuración como [elementos de proyecto de MSBuild](/visualstudio/msbuild/common-msbuild-project-items) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="96fa9-200">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="96fa9-201">La aplicación de ejemplo mueve sus archivos de configuración de aplicación JSON y *hostsettings.json* con el elemento **&lt;Content:&gt;** siguiente:</span><span class="sxs-lookup"><span data-stu-id="96fa9-201">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="96fa9-202">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="96fa9-202">ConfigureServices</span></span>

<span data-ttu-id="96fa9-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-203">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="96fa9-204">Se puede llamar varias veces a `ConfigureServices` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96fa9-204">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="96fa9-205">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="96fa9-205">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="96fa9-206">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="96fa9-206">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="96fa9-207">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96fa9-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="96fa9-208">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="96fa9-208">ConfigureLogging</span></span>

<span data-ttu-id="96fa9-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) agrega un delegado para configurar el [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) proporcionado.</span><span class="sxs-lookup"><span data-stu-id="96fa9-209">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="96fa9-210">Se puede llamar varias veces a `ConfigureLogging` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96fa9-210">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="96fa9-211">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="96fa9-211">UseConsoleLifetime</span></span>

<span data-ttu-id="96fa9-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escucha `Ctrl+C`/SIGINT o SIGTERM y llama a [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="96fa9-212">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="96fa9-213">`UseConsoleLifetime` desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="96fa9-213">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="96fa9-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="96fa9-214">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="96fa9-215">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="96fa9-215">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="96fa9-216">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="96fa9-216">Container configuration</span></span>

<span data-ttu-id="96fa9-217">Para permitir la conexión a otros contenedores, el host puede aceptar [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="96fa9-217">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="96fa9-218">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="96fa9-218">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="96fa9-219">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-219">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="96fa9-220">La configuración personalizada del contenedor está administrada por el método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="96fa9-220">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="96fa9-221">`ConfigureContainer` proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="96fa9-221">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="96fa9-222">Se puede llamar varias veces a `ConfigureContainer` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="96fa9-222">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="96fa9-223">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96fa9-223">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="96fa9-224">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="96fa9-224">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="96fa9-225">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="96fa9-225">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="96fa9-226">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="96fa9-226">Extensibility</span></span>

<span data-ttu-id="96fa9-227">La extensibilidad de host se realiza con métodos de extensión en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="96fa9-227">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="96fa9-228">El ejemplo siguiente muestra cómo un método de extensión extiende una implementación de `IHostBuilder` con [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="96fa9-228">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="96fa9-229">El método de extensión (en otra parte de la aplicación) registra un `IHostedService` de RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="96fa9-229">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="96fa9-230">Administración del host</span><span class="sxs-lookup"><span data-stu-id="96fa9-230">Manage the host</span></span>

<span data-ttu-id="96fa9-231">La implementación de [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) es la responsable de iniciar y detener las implementaciones de `IHostedService` que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="96fa9-231">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="96fa9-232">Run</span><span class="sxs-lookup"><span data-stu-id="96fa9-232">Run</span></span>

<span data-ttu-id="96fa9-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="96fa9-233">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="96fa9-234">RunAsync</span><span class="sxs-lookup"><span data-stu-id="96fa9-234">RunAsync</span></span>

<span data-ttu-id="96fa9-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) inicia la aplicación y devuelve `Task`, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="96fa9-235">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="96fa9-236">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="96fa9-236">RunConsoleAsync</span></span>

<span data-ttu-id="96fa9-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre `Ctrl+C`/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="96fa9-237">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="96fa9-238">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="96fa9-238">Start and StopAsync</span></span>

<span data-ttu-id="96fa9-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="96fa9-239">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="96fa9-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="96fa9-240">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="96fa9-241">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="96fa9-241">StartAsync and StopAsync</span></span>

<span data-ttu-id="96fa9-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-242">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="96fa9-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-243">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="96fa9-244">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="96fa9-244">WaitForShutdown</span></span>

<span data-ttu-id="96fa9-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se desencadena mediante [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escucha `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="96fa9-245">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="96fa9-246">`WaitForShutdown` llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="96fa9-246">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="96fa9-247">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="96fa9-247">WaitForShutdownAsync</span></span>

<span data-ttu-id="96fa9-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) devuelve `Task`, que se completa cuando se desencadena el cierre a través del token determinado y llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="96fa9-248">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="96fa9-249">Control externo</span><span class="sxs-lookup"><span data-stu-id="96fa9-249">External control</span></span>

<span data-ttu-id="96fa9-250">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="96fa9-250">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="96fa9-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) se llama al inicio de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="96fa9-251">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="96fa9-252">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="96fa9-252">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="96fa9-253">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="96fa9-253">IHostingEnvironment interface</span></span>

<span data-ttu-id="96fa9-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-254">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="96fa9-255">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="96fa9-255">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="96fa9-256">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="96fa9-256">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="96fa9-257">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="96fa9-257">IApplicationLifetime interface</span></span>

<span data-ttu-id="96fa9-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="96fa9-258">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="96fa9-259">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="96fa9-259">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="96fa9-260">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="96fa9-260">Cancellation Token</span></span> | <span data-ttu-id="96fa9-261">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="96fa9-261">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="96fa9-262">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="96fa9-262">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="96fa9-263">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="96fa9-263">The host has fully started.</span></span> |
| [<span data-ttu-id="96fa9-264">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="96fa9-264">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="96fa9-265">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="96fa9-265">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="96fa9-266">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="96fa9-266">All requests should be processed.</span></span> <span data-ttu-id="96fa9-267">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="96fa9-267">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="96fa9-268">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="96fa9-268">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="96fa9-269">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="96fa9-269">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="96fa9-270">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="96fa9-270">Requests may still be processing.</span></span> <span data-ttu-id="96fa9-271">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="96fa9-271">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="96fa9-272">Inserción de constructor del servicio `IApplicationLifetime` en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="96fa9-272">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="96fa9-273">En la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) se usa la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de `IHostedService`) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="96fa9-273">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="96fa9-274">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="96fa9-274">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="96fa9-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="96fa9-275">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="96fa9-276">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="96fa9-276">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="96fa9-277">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="96fa9-277">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="96fa9-278">Ejemplos de hospedaje de repositorios en GitHub</span><span class="sxs-lookup"><span data-stu-id="96fa9-278">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
