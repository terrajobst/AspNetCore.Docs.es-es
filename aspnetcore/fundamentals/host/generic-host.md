---
title: Host genérico de .NET
author: guardrex
description: Obtenga información sobre el host genérico en .NET, que es responsable de la administración de inicio y duración de la aplicación.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: a851f2faf13792b2c232c124371d07710ae1fce3
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734476"
---
# <a name="net-generic-host"></a><span data-ttu-id="3023e-103">Host genérico de .NET</span><span class="sxs-lookup"><span data-stu-id="3023e-103">.NET Generic Host</span></span>

<span data-ttu-id="3023e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3023e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3023e-105">Las aplicaciones .NET configuran e inician un *host*.</span><span class="sxs-lookup"><span data-stu-id="3023e-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="3023e-106">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3023e-107">En este tema se trata el host genérico de ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), muy útil para hospedar aplicaciones que no procesan solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="3023e-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="3023e-108">Para la cobertura del host de web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), consulte el tema [Host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="3023e-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="3023e-109">El objetivo del host genérico es desacoplar la canalización HTTP de la API host web para habilitar una variedad de escenarios de host.</span><span class="sxs-lookup"><span data-stu-id="3023e-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="3023e-110">Se trata de la mensajería, tareas en segundo plano y otras cargas de trabajo no HTTP basadas en la ventaja de host genérico de capacidades transversales, como la configuración, la inserción de dependencias (DI) y el registro.</span><span class="sxs-lookup"><span data-stu-id="3023e-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="3023e-111">El host genérico es una novedad de ASP.NET Core 2.1 y no es adecuado para escenarios de hospedaje web.</span><span class="sxs-lookup"><span data-stu-id="3023e-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="3023e-112">Para escenarios de hospedaje web, use el [host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="3023e-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="3023e-113">El host genérico se está desarrollando para reemplazar el host web en una próxima versión y actuar como la API del host principal tanto en escenarios que sean del tipo HTTP como los que no.</span><span class="sxs-lookup"><span data-stu-id="3023e-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="3023e-114">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3023e-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3023e-115">Al ejecutar la aplicación de ejemplo en [Visual Studio Code](https://code.visualstudio.com/), use un *terminal integrado o externo*.</span><span class="sxs-lookup"><span data-stu-id="3023e-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="3023e-116">No ejecute el ejemplo en `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="3023e-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="3023e-117">Para establecer la consola en Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="3023e-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="3023e-118">Abra el archivo *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="3023e-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="3023e-119">En la configuración de **.NET Core Launch (console)**, busque la entrada **console**.</span><span class="sxs-lookup"><span data-stu-id="3023e-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="3023e-120">Establezca el valor en `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="3023e-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="3023e-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="3023e-121">Introduction</span></span>

<span data-ttu-id="3023e-122">La biblioteca de host genérico está disponible en el [espacio de nombres de Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) y la proporciona el [paquete Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="3023e-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="3023e-123">El paquete `Microsoft.Extensions.Hosting` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="3023e-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="3023e-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) es el punto de entrada para la ejecución de código.</span><span class="sxs-lookup"><span data-stu-id="3023e-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="3023e-125">Cada implementación de `IHostedService` se ejecuta en el orden de [registro del servicio en ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="3023e-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="3023e-126">Se llama a [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) en cada `IHostedService` cuando se inicia el host, y se llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) en orden inverso del registro cuando el host se cierra de forma estable.</span><span class="sxs-lookup"><span data-stu-id="3023e-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3023e-127">Configuración de un host</span><span class="sxs-lookup"><span data-stu-id="3023e-127">Set up a host</span></span>

<span data-ttu-id="3023e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) es el componente principal que usan las bibliotecas y aplicaciones para inicializar, compilar y ejecutar el host:</span><span class="sxs-lookup"><span data-stu-id="3023e-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="3023e-129">Configuración de host</span><span class="sxs-lookup"><span data-stu-id="3023e-129">Host configuration</span></span>

<span data-ttu-id="3023e-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) se basa en los siguientes métodos para establecer los valores de configuración del host:</span><span class="sxs-lookup"><span data-stu-id="3023e-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="3023e-131">Generador de configuración</span><span class="sxs-lookup"><span data-stu-id="3023e-131">Configuration builder</span></span>
* <span data-ttu-id="3023e-132">Configuración del método de extensión</span><span class="sxs-lookup"><span data-stu-id="3023e-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="3023e-133">Generador de configuración</span><span class="sxs-lookup"><span data-stu-id="3023e-133">Configuration builder</span></span>

<span data-ttu-id="3023e-134">La configuración del generador de host se crea mediante una llamada a [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3023e-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="3023e-135">`ConfigureHostConfiguration` usa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para el host.</span><span class="sxs-lookup"><span data-stu-id="3023e-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="3023e-136">El generador de configuración inicializa [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) para su uso en el proceso de compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="3023e-137">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3023e-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3023e-138">El host usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="3023e-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="3023e-139">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3023e-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="3023e-140">Configuración de `HostBuilder` de ejemplo con `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="3023e-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="3023e-141">El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="3023e-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3023e-142">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="3023e-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="3023e-143">El método `AddConfiguration` espera que las claves coincidan con las claves `HostBuilder` (por ejemplo, `environment`).</span><span class="sxs-lookup"><span data-stu-id="3023e-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="3023e-144">La presencia del nombre de sección en las claves evita que los valores de la sección configuren el host.</span><span class="sxs-lookup"><span data-stu-id="3023e-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="3023e-145">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="3023e-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3023e-146">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="3023e-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="3023e-147">Configuración del método de extensión</span><span class="sxs-lookup"><span data-stu-id="3023e-147">Extension method configuration</span></span>

<span data-ttu-id="3023e-148">Se llama a métodos de extensión en la implementación de `IHostBuilder` para configurar la raíz de contenido y el entorno.</span><span class="sxs-lookup"><span data-stu-id="3023e-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="3023e-149">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="3023e-149">Content Root</span></span>

<span data-ttu-id="3023e-150">Esta configuración determina la ubicación en la que el host comienza a buscar archivos de contenido.</span><span class="sxs-lookup"><span data-stu-id="3023e-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="3023e-151">**Clave**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="3023e-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="3023e-152">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="3023e-152">**Type**: *string*</span></span>  
<span data-ttu-id="3023e-153">**Valor predeterminado**: la carpeta donde se encuentra el ensamblado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="3023e-154">**Establecer mediante**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="3023e-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="3023e-155">**Variable de entorno**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="3023e-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="3023e-156">Si no existe la ruta de acceso, el host no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="3023e-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="3023e-157">Entorno</span><span class="sxs-lookup"><span data-stu-id="3023e-157">Environment</span></span>

<span data-ttu-id="3023e-158">Establece el [entorno](xref:fundamentals/environments) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="3023e-159">**Clave**: environment</span><span class="sxs-lookup"><span data-stu-id="3023e-159">**Key**: environment</span></span>  
<span data-ttu-id="3023e-160">**Tipo**: *cadena*</span><span class="sxs-lookup"><span data-stu-id="3023e-160">**Type**: *string*</span></span>  
<span data-ttu-id="3023e-161">**Valor predeterminado**: producción</span><span class="sxs-lookup"><span data-stu-id="3023e-161">**Default**: Production</span></span>  
<span data-ttu-id="3023e-162">**Establecer mediante**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="3023e-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="3023e-163">**Variable de entorno**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="3023e-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="3023e-164">El entorno se puede establecer en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="3023e-164">The environment can be set to any value.</span></span> <span data-ttu-id="3023e-165">Los valores definidos por el marco son `Development`, `Staging` y `Production`.</span><span class="sxs-lookup"><span data-stu-id="3023e-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="3023e-166">Los valores no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3023e-166">Values aren't case sensitive.</span></span> <span data-ttu-id="3023e-167">De forma predeterminada, el *entorno* se lee desde la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="3023e-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="3023e-168">Cuando se usa [Visual Studio](https://www.visualstudio.com/), las variables de entorno se pueden establecer en el archivo *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3023e-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="3023e-169">Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3023e-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="3023e-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="3023e-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="3023e-171">La configuración del generador de la aplicación se crea mediante una llamada a [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) en la implementación de [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="3023e-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="3023e-172">`ConfigureAppConfiguration` usa un [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) para crear un [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="3023e-173">Se puede llamar varias veces a `ConfigureAppConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3023e-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3023e-174">La aplicación usa cualquier opción que establece un valor en último lugar.</span><span class="sxs-lookup"><span data-stu-id="3023e-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="3023e-175">La configuración creada por `ConfigureAppConfiguration` está disponible en [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) para las operaciones posteriores y en [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="3023e-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="3023e-176">Configuración de aplicación de ejemplo con `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="3023e-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="3023e-177">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3023e-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="3023e-178">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="3023e-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="3023e-179">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="3023e-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="3023e-180">El método de extensión [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) no es capaz de analizar actualmente una sección de configuración devuelta por [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (por ejemplo, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="3023e-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="3023e-181">El método `GetSection` filtra las claves de configuración a la sección solicitada, pero deja el nombre de sección en las claves (por ejemplo, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="3023e-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="3023e-182">El método `AddConfiguration` espera una coincidencia exacta con las claves de configuración (por ejemplo, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="3023e-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="3023e-183">La presencia del nombre de sección en las claves evita que los valores de la sección configuren la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="3023e-184">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="3023e-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="3023e-185">Para obtener más información y soluciones alternativas, consulte [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (Pasar la sección de configuración a WebHostBuilder.UseConfiguration usa claves completas).</span><span class="sxs-lookup"><span data-stu-id="3023e-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="3023e-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="3023e-186">ConfigureServices</span></span>

<span data-ttu-id="3023e-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) agrega los servicios al contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3023e-188">Se puede llamar varias veces a `ConfigureServices` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3023e-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="3023e-189">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="3023e-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="3023e-190">Para más información, consulte el tema [Tareas en segundo plano con servicios hospedados](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="3023e-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="3023e-191">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) usa el método de extensión `AddHostedService` para agregar un servicio para eventos de duración, `LifetimeEventsHostedService`, y una tarea en segundo plano programada, `TimedHostedService`, a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3023e-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="3023e-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="3023e-192">ConfigureLogging</span></span>

<span data-ttu-id="3023e-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) agrega un delegado para configurar el [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) proporcionado.</span><span class="sxs-lookup"><span data-stu-id="3023e-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="3023e-194">Se puede llamar varias veces a `ConfigureLogging` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3023e-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="3023e-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="3023e-195">UseConsoleLifetime</span></span>

<span data-ttu-id="3023e-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) escucha `Ctrl+C`/SIGINT o SIGTERM y llama a [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) para iniciar el proceso de cierre.</span><span class="sxs-lookup"><span data-stu-id="3023e-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="3023e-197">`UseConsoleLifetime` desbloquea extensiones como [RunAsync](#runasync) y [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="3023e-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="3023e-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) ya está registrado previamente como la implementación de duración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3023e-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="3023e-199">Se usa la última duración registrada.</span><span class="sxs-lookup"><span data-stu-id="3023e-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="3023e-200">Configuración del contenedor</span><span class="sxs-lookup"><span data-stu-id="3023e-200">Container configuration</span></span>

<span data-ttu-id="3023e-201">Para permitir la conexión a otros contenedores, el host puede aceptar [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="3023e-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="3023e-202">Proporcionar un generador no forma parte del registro de contenedor DI, sino que es un host intrínseco utilizado para crear el contenedor DI determinado.</span><span class="sxs-lookup"><span data-stu-id="3023e-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="3023e-203">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) invalida el generador predeterminado utilizado para crear el proveedor de servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="3023e-204">La configuración personalizada del contenedor está administrada por el método [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="3023e-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="3023e-205">`ConfigureContainer` proporciona una experiencia fuertemente tipada para configurar el contenedor sobre la API de host subyacente.</span><span class="sxs-lookup"><span data-stu-id="3023e-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="3023e-206">Se puede llamar varias veces a `ConfigureContainer` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="3023e-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="3023e-207">Crear un contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3023e-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="3023e-208">Proporcionar un generador de contenedor de servicio:</span><span class="sxs-lookup"><span data-stu-id="3023e-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="3023e-209">Usar el generador y configurar el contenedor de servicio personalizado de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3023e-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="3023e-210">Extensibilidad</span><span class="sxs-lookup"><span data-stu-id="3023e-210">Extensibility</span></span>

<span data-ttu-id="3023e-211">La extensibilidad de host se realiza con métodos de extensión en `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3023e-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="3023e-212">El ejemplo siguiente muestra cómo un método de extensión extiende una implementación de `IHostBuilder` con [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="3023e-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="3023e-213">El método de extensión (en otra parte de la aplicación) registra un `IHostedService` de RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="3023e-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="3023e-214">Administración del host</span><span class="sxs-lookup"><span data-stu-id="3023e-214">Manage the host</span></span>

<span data-ttu-id="3023e-215">La implementación de [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) es la responsable de iniciar y detener las implementaciones de `IHostedService` que están registradas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="3023e-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="3023e-216">Run</span><span class="sxs-lookup"><span data-stu-id="3023e-216">Run</span></span>

<span data-ttu-id="3023e-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) inicia la aplicación y bloquea el subproceso que realiza la llamada hasta que se cierre el host:</span><span class="sxs-lookup"><span data-stu-id="3023e-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="3023e-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="3023e-218">RunAsync</span></span>

<span data-ttu-id="3023e-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) inicia la aplicación y devuelve `Task`, que se completa cuando se desencadena el token de cancelación o el cierre:</span><span class="sxs-lookup"><span data-stu-id="3023e-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="3023e-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="3023e-220">RunConsoleAsync</span></span>

<span data-ttu-id="3023e-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) habilita la compatibilidad de la consola, compila e inicia el host y espera a que se cierre `Ctrl+C`/SIGINT o SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="3023e-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="3023e-222">Start y StopAsync</span><span class="sxs-lookup"><span data-stu-id="3023e-222">Start and StopAsync</span></span>

<span data-ttu-id="3023e-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) inicia el host de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="3023e-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="3023e-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) intenta detener el host en el tiempo de espera proporcionado.</span><span class="sxs-lookup"><span data-stu-id="3023e-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="3023e-225">StartAsync y StopAsync</span><span class="sxs-lookup"><span data-stu-id="3023e-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="3023e-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="3023e-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) detiene la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="3023e-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="3023e-228">WaitForShutdown</span></span>

<span data-ttu-id="3023e-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) se desencadena mediante [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), como [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (escucha `Ctrl+C`/SIGINT o SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="3023e-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="3023e-230">`WaitForShutdown` llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="3023e-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="3023e-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="3023e-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="3023e-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) devuelve `Task`, que se completa cuando se desencadena el cierre a través del token determinado y llama a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="3023e-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="3023e-233">Control externo</span><span class="sxs-lookup"><span data-stu-id="3023e-233">External control</span></span>

<span data-ttu-id="3023e-234">El control externo del host se puede lograr mediante métodos a los que se pueda llamar de forma externa:</span><span class="sxs-lookup"><span data-stu-id="3023e-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="3023e-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) se llama al inicio de [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), que espera hasta que se complete antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="3023e-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="3023e-236">Esto se puede usar para retrasar el inicio hasta que lo indique un evento externo.</span><span class="sxs-lookup"><span data-stu-id="3023e-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="3023e-237">Interfaz IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="3023e-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="3023e-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) proporciona información sobre el entorno de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="3023e-239">Use [inserción de constructores](xref:fundamentals/dependency-injection) para obtener `IHostingEnvironment` a fin de utilizar sus propiedades y métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="3023e-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="3023e-240">Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3023e-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="3023e-241">Interfaz IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="3023e-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="3023e-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) permite actividades posteriores al inicio y cierre, incluidas las solicitudes de cierre estable.</span><span class="sxs-lookup"><span data-stu-id="3023e-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="3023e-243">Hay tres propiedades en la interfaz que son tokens de cancelación usados para registrar métodos `Action` que definen los eventos de inicio y apagado.</span><span class="sxs-lookup"><span data-stu-id="3023e-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="3023e-244">Token de cancelación</span><span class="sxs-lookup"><span data-stu-id="3023e-244">Cancellation Token</span></span> | <span data-ttu-id="3023e-245">Se desencadena cuando&#8230;</span><span class="sxs-lookup"><span data-stu-id="3023e-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="3023e-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="3023e-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="3023e-247">El host se ha iniciado completamente.</span><span class="sxs-lookup"><span data-stu-id="3023e-247">The host has fully started.</span></span> |
| [<span data-ttu-id="3023e-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="3023e-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="3023e-249">El host está completando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="3023e-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="3023e-250">Deben procesarse todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3023e-250">All requests should be processed.</span></span> <span data-ttu-id="3023e-251">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="3023e-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="3023e-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="3023e-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="3023e-253">El host está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="3023e-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="3023e-254">Puede que todavía se estén procesando las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3023e-254">Requests may still be processing.</span></span> <span data-ttu-id="3023e-255">El apagado se bloquea hasta que se complete este evento.</span><span class="sxs-lookup"><span data-stu-id="3023e-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="3023e-256">Inserción de constructor del servicio `IApplicationLifetime` en cualquier clase.</span><span class="sxs-lookup"><span data-stu-id="3023e-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="3023e-257">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) utiliza la inserción de constructor en una clase `LifetimeEventsHostedService` (una implementación de `IHostedService`) para registrar los eventos.</span><span class="sxs-lookup"><span data-stu-id="3023e-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="3023e-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="3023e-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="3023e-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) solicita la terminación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3023e-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="3023e-260">La siguiente clase usa `StopApplication` para cerrar de forma estable una aplicación cuando se llama al método `Shutdown` de esa clase:</span><span class="sxs-lookup"><span data-stu-id="3023e-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="3023e-261">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3023e-261">Additional resources</span></span>

* [<span data-ttu-id="3023e-262">Tareas en segundo plano con servicios hospedados</span><span class="sxs-lookup"><span data-stu-id="3023e-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="3023e-263">Ejemplos de hospedaje de repositorios en GitHub</span><span class="sxs-lookup"><span data-stu-id="3023e-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
