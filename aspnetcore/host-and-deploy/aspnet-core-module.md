---
title: Referencia de configuración del módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ad73d89ffa3a8a3625c6e248efaad821e1b4d0a
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121562"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="23c86-103">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23c86-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="23c86-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="23c86-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="23c86-105">En este documento se proporcionan instrucciones sobre cómo configurar el módulo ASP.NET Core para hospedar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c86-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="23c86-106">Puede encontrar una introducción al módulo ASP.NET Core e instrucciones de instalación en el artículo de [introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="23c86-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="23c86-107">Modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="23c86-107">Hosting model</span></span>

<span data-ttu-id="23c86-108">En las aplicaciones con .NET Core 2.2 o posterior, el módulo admite un modelo de hospedaje en proceso para un mejor rendimiento en comparación con el hospedaje de proxy inverso (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="23c86-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="23c86-109">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="23c86-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="23c86-110">El hospedaje en proceso es opcional para las aplicaciones existentes pero, para las plantillas [dotnet new](/dotnet/core/tools/dotnet-new), este modelo es el predeterminado para todos los escenarios de IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="23c86-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="23c86-111">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación (por ejemplo, *MiAplicación.csproj*) con el valor `InProcess` (el hospedaje fuera de proceso se establece con `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="23c86-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `InProcess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="23c86-112">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="23c86-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="23c86-113">En lugar del servidor HTTP de IIS (`IISHttpServer`) se usa el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="23c86-113">IIS HTTP Server (`IISHttpServer`) is used instead of the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="23c86-114">El servidor HTTP de IIS (`IISHttpServer`) es otra implementación de <xref:Microsoft.AspNetCore.Hosting.Server.IServer> que convierte las solicitudes nativas de IIS en administradas de ASP.NET Core para que la aplicación las procese.</span><span class="sxs-lookup"><span data-stu-id="23c86-114">IIS HTTP Server (`IISHttpServer`) is another <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation that converts IIS native requests into ASP.NET Core managed requests for processing by the app.</span></span>

* <span data-ttu-id="23c86-115">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="23c86-116">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="23c86-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="23c86-117">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-117">Use one app pool per app.</span></span>

* <span data-ttu-id="23c86-118">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="23c86-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="23c86-119">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="23c86-120">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="23c86-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="23c86-121">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="23c86-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="23c86-122">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="23c86-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="23c86-123">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="23c86-123">Client disconnects are detected.</span></span> <span data-ttu-id="23c86-124">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="23c86-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="23c86-125"><xref:System.IO.Directory.GetCurrentDirectory*> devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="23c86-125"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="23c86-126">Para conocer el código de ejemplo que establece el directorio actual de la aplicación, consulte la información sobre la [clase CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="23c86-126">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="23c86-127">Llame al método `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="23c86-127">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="23c86-128">Las llamadas subsiguientes a <xref:System.IO.Directory.GetCurrentDirectory*> proporcionan el directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-128">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="23c86-129">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="23c86-129">Hosting model changes</span></span>

<span data-ttu-id="23c86-130">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="23c86-130">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="23c86-131">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="23c86-131">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="23c86-132">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="23c86-132">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="23c86-133">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="23c86-133">Process name</span></span>

<span data-ttu-id="23c86-134">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="23c86-134">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="23c86-135">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="23c86-135">Configuration with web.config</span></span>

<span data-ttu-id="23c86-136">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-136">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="23c86-137">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="23c86-137">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="23c86-138">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="23c86-138">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="23c86-139">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="23c86-139">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="23c86-140">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="23c86-140">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="23c86-141">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="23c86-141">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="23c86-142">Para obtener información sobre la configuración de aplicaciones secundarias de IIS, consulte <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="23c86-142">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="23c86-143">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="23c86-143">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="23c86-144">Atributo</span><span class="sxs-lookup"><span data-stu-id="23c86-144">Attribute</span></span> | <span data-ttu-id="23c86-145">Descripción</span><span class="sxs-lookup"><span data-stu-id="23c86-145">Description</span></span> | <span data-ttu-id="23c86-146">Default</span><span class="sxs-lookup"><span data-stu-id="23c86-146">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23c86-147">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-147">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-148">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-148">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23c86-149">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-149">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-150">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="23c86-150">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23c86-151">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-151">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-152">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-152">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23c86-153">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-153">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="23c86-154">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-154">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-155">Especifica el modelo de hospedaje como en proceso (`InProcess`) o fuera de proceso (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="23c86-155">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="23c86-156">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-156">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-157">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-157">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="23c86-158">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="23c86-158">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="23c86-159">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-159">Default: `1`</span></span><br><span data-ttu-id="23c86-160">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-160">Min: `1`</span></span><br><span data-ttu-id="23c86-161">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="23c86-161">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="23c86-162">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="23c86-162">Required string attribute.</span></span></p><p><span data-ttu-id="23c86-163">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="23c86-163">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23c86-164">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="23c86-164">Relative paths are supported.</span></span> <span data-ttu-id="23c86-165">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-165">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23c86-166">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-166">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-167">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-167">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23c86-168">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-168">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="23c86-169">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-169">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="23c86-170">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-170">Default: `10`</span></span><br><span data-ttu-id="23c86-171">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-171">Min: `0`</span></span><br><span data-ttu-id="23c86-172">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="23c86-172">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23c86-173">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-173">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23c86-174">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="23c86-174">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23c86-175">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="23c86-175">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="23c86-176">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-176">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="23c86-177">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-177">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="23c86-178">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-178">Default: `00:02:00`</span></span><br><span data-ttu-id="23c86-179">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-179">Min: `00:00:00`</span></span><br><span data-ttu-id="23c86-180">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-180">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23c86-181">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-182">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23c86-182">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23c86-183">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-183">Default: `10`</span></span><br><span data-ttu-id="23c86-184">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-184">Min: `0`</span></span><br><span data-ttu-id="23c86-185">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="23c86-185">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23c86-186">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-186">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-187">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="23c86-187">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23c86-188">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-188">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23c86-189">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="23c86-189">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="23c86-190">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="23c86-190">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="23c86-191">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="23c86-191">Default: `120`</span></span><br><span data-ttu-id="23c86-192">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-192">Min: `0`</span></span><br><span data-ttu-id="23c86-193">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="23c86-193">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23c86-194">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-194">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-195">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-195">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23c86-196">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-196">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-197">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-197">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23c86-198">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-198">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23c86-199">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="23c86-199">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23c86-200">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="23c86-200">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23c86-201">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-201">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23c86-202">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="23c86-202">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="23c86-203">Atributo</span><span class="sxs-lookup"><span data-stu-id="23c86-203">Attribute</span></span> | <span data-ttu-id="23c86-204">Descripción</span><span class="sxs-lookup"><span data-stu-id="23c86-204">Description</span></span> | <span data-ttu-id="23c86-205">Default</span><span class="sxs-lookup"><span data-stu-id="23c86-205">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23c86-206">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-206">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-207">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-207">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23c86-208">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-208">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-209">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="23c86-209">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23c86-210">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-210">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-211">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-211">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23c86-212">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-212">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="23c86-213">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-214">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-214">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="23c86-215">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-215">Default: `1`</span></span><br><span data-ttu-id="23c86-216">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-216">Min: `1`</span></span><br><span data-ttu-id="23c86-217">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="23c86-217">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="23c86-218">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="23c86-218">Required string attribute.</span></span></p><p><span data-ttu-id="23c86-219">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="23c86-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23c86-220">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="23c86-220">Relative paths are supported.</span></span> <span data-ttu-id="23c86-221">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23c86-222">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-223">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23c86-224">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="23c86-225">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-225">Default: `10`</span></span><br><span data-ttu-id="23c86-226">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-226">Min: `0`</span></span><br><span data-ttu-id="23c86-227">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="23c86-227">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23c86-228">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-228">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23c86-229">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="23c86-229">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23c86-230">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="23c86-230">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="23c86-231">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-231">Default: `00:02:00`</span></span><br><span data-ttu-id="23c86-232">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-232">Min: `00:00:00`</span></span><br><span data-ttu-id="23c86-233">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23c86-234">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-235">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23c86-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23c86-236">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-236">Default: `10`</span></span><br><span data-ttu-id="23c86-237">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-237">Min: `0`</span></span><br><span data-ttu-id="23c86-238">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="23c86-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23c86-239">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-240">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="23c86-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23c86-241">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23c86-242">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="23c86-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="23c86-243">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="23c86-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="23c86-244">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="23c86-244">Default: `120`</span></span><br><span data-ttu-id="23c86-245">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-245">Min: `0`</span></span><br><span data-ttu-id="23c86-246">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="23c86-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23c86-247">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-248">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23c86-249">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-249">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-250">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23c86-251">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23c86-252">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="23c86-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23c86-253">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="23c86-253">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23c86-254">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23c86-255">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="23c86-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="23c86-256">Atributo</span><span class="sxs-lookup"><span data-stu-id="23c86-256">Attribute</span></span> | <span data-ttu-id="23c86-257">Descripción</span><span class="sxs-lookup"><span data-stu-id="23c86-257">Description</span></span> | <span data-ttu-id="23c86-258">Default</span><span class="sxs-lookup"><span data-stu-id="23c86-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="23c86-259">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-259">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-260">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="23c86-261">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-262">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="23c86-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="23c86-263">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-264">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="23c86-265">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="23c86-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="23c86-266">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-267">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="23c86-268">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-268">Default: `1`</span></span><br><span data-ttu-id="23c86-269">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="23c86-269">Min: `1`</span></span><br><span data-ttu-id="23c86-270">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="23c86-270">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="23c86-271">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="23c86-271">Required string attribute.</span></span></p><p><span data-ttu-id="23c86-272">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="23c86-272">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="23c86-273">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="23c86-273">Relative paths are supported.</span></span> <span data-ttu-id="23c86-274">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-274">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="23c86-275">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-276">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-276">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="23c86-277">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="23c86-277">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="23c86-278">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-278">Default: `10`</span></span><br><span data-ttu-id="23c86-279">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-279">Min: `0`</span></span><br><span data-ttu-id="23c86-280">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="23c86-280">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="23c86-281">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-281">Optional timespan attribute.</span></span></p><p><span data-ttu-id="23c86-282">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="23c86-282">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="23c86-283">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="23c86-283">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="23c86-284">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-284">Default: `00:02:00`</span></span><br><span data-ttu-id="23c86-285">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-285">Min: `00:00:00`</span></span><br><span data-ttu-id="23c86-286">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="23c86-286">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="23c86-287">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-288">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23c86-288">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="23c86-289">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="23c86-289">Default: `10`</span></span><br><span data-ttu-id="23c86-290">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-290">Min: `0`</span></span><br><span data-ttu-id="23c86-291">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="23c86-291">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="23c86-292">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-292">Optional integer attribute.</span></span></p><p><span data-ttu-id="23c86-293">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="23c86-293">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="23c86-294">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-294">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="23c86-295">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="23c86-295">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="23c86-296">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="23c86-296">Default: `120`</span></span><br><span data-ttu-id="23c86-297">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="23c86-297">Min: `0`</span></span><br><span data-ttu-id="23c86-298">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="23c86-298">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="23c86-299">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-299">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="23c86-300">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-300">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="23c86-301">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="23c86-301">Optional string attribute.</span></span></p><p><span data-ttu-id="23c86-302">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="23c86-302">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="23c86-303">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="23c86-303">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="23c86-304">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="23c86-304">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="23c86-305">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="23c86-305">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23c86-306">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="23c86-306">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="23c86-307">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="23c86-307">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="23c86-308">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="23c86-308">Setting environment variables</span></span>

<span data-ttu-id="23c86-309">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="23c86-309">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="23c86-310">Especifique una variable de entorno con el elemento secundario `environmentVariable` de un elemento de la colección `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="23c86-310">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="23c86-311">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="23c86-311">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="23c86-312">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="23c86-312">The following example sets two environment variables.</span></span> <span data-ttu-id="23c86-313">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="23c86-313">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="23c86-314">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-314">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="23c86-315">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-315">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="23c86-316">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="23c86-316">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="23c86-317">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="23c86-317">app_offline.htm</span></span>

<span data-ttu-id="23c86-318">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="23c86-318">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="23c86-319">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="23c86-319">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="23c86-320">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="23c86-320">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="23c86-321">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-321">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c86-322">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="23c86-322">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="23c86-323">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-323">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="23c86-324">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="23c86-324">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c86-325">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-325">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="23c86-326">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="23c86-326">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="23c86-327">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="23c86-327">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="23c86-328">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="23c86-328">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="23c86-329">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="23c86-329">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="23c86-330">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="23c86-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="23c86-331">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="23c86-331">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="23c86-332">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="23c86-332">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="23c86-333">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="23c86-333">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="23c86-335">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="23c86-335">Log creation and redirection</span></span>

<span data-ttu-id="23c86-336">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="23c86-336">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="23c86-337">Las carpetas de la ruta de acceso `stdoutLogFile` debe estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="23c86-337">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="23c86-338">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="23c86-338">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="23c86-339">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="23c86-339">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="23c86-340">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="23c86-340">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="23c86-341">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-341">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="23c86-342">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="23c86-342">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="23c86-343">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="23c86-343">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="23c86-344">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="23c86-344">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="23c86-345">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="23c86-345">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="23c86-346">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="23c86-346">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="23c86-347">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="23c86-347">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c86-348">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="23c86-348">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="23c86-349">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="23c86-349">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="23c86-350">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="23c86-350">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="23c86-351">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="23c86-351">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="23c86-352">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="23c86-352">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="23c86-353">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="23c86-353">Enhanced diagnostic logs</span></span>

<span data-ttu-id="23c86-354">El módulo ASP.NET Core que se proporciona es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="23c86-354">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="23c86-355">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="23c86-355">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="23c86-356">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-356">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="23c86-357">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="23c86-357">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="23c86-358">ERROR</span><span class="sxs-lookup"><span data-stu-id="23c86-358">ERROR</span></span>
* <span data-ttu-id="23c86-359">WARNING</span><span class="sxs-lookup"><span data-stu-id="23c86-359">WARNING</span></span>
* <span data-ttu-id="23c86-360">INFO</span><span class="sxs-lookup"><span data-stu-id="23c86-360">INFO</span></span>
* <span data-ttu-id="23c86-361">TRACE</span><span class="sxs-lookup"><span data-stu-id="23c86-361">TRACE</span></span>

<span data-ttu-id="23c86-362">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="23c86-362">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="23c86-363">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="23c86-363">CONSOLE</span></span>
* <span data-ttu-id="23c86-364">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="23c86-364">EVENTLOG</span></span>
* <span data-ttu-id="23c86-365">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="23c86-365">FILE</span></span>

<span data-ttu-id="23c86-366">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="23c86-366">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="23c86-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="23c86-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="23c86-368">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="23c86-368">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="23c86-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="23c86-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="23c86-370">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="23c86-370">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="23c86-371">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="23c86-371">The size of the log isn't limited.</span></span> <span data-ttu-id="23c86-372">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="23c86-372">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="23c86-373">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="23c86-373">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="23c86-374">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="23c86-374">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="23c86-375">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="23c86-375">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="23c86-376">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="23c86-376">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="23c86-377">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="23c86-377">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="23c86-378">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="23c86-378">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="23c86-379">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="23c86-379">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="23c86-380">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="23c86-380">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="23c86-381">El token de emparejamiento también se establece en un encabezado (`MSAspNetCoreToken`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="23c86-381">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="23c86-382">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="23c86-382">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="23c86-383">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="23c86-383">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="23c86-384">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="23c86-384">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="23c86-385">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="23c86-385">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="23c86-386">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="23c86-386">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="23c86-387">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="23c86-387">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="23c86-388">Dado que la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador recibe un error de acceso denegado al intentar configurar los valores del módulo en *applicationHost.config* en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="23c86-388">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="23c86-389">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="23c86-389">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="23c86-390">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="23c86-390">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="23c86-391">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="23c86-391">Run the installer.</span></span>
1. <span data-ttu-id="23c86-392">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="23c86-392">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="23c86-393">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="23c86-393">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="23c86-394">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="23c86-394">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="23c86-395">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="23c86-395">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="23c86-396">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="23c86-396">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="23c86-397">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="23c86-397">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="23c86-398">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="23c86-398">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="23c86-399">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="23c86-399">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="23c86-400">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="23c86-400">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="23c86-401">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="23c86-401">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="23c86-402">Module</span><span class="sxs-lookup"><span data-stu-id="23c86-402">Module</span></span>

<span data-ttu-id="23c86-403">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="23c86-403">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="23c86-404">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-404">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="23c86-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23c86-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="23c86-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="23c86-408">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="23c86-408">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="23c86-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="23c86-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23c86-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="23c86-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="23c86-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="23c86-413">Schema</span><span class="sxs-lookup"><span data-stu-id="23c86-413">Schema</span></span>

<span data-ttu-id="23c86-414">**IIS**</span><span class="sxs-lookup"><span data-stu-id="23c86-414">**IIS**</span></span>

   * <span data-ttu-id="23c86-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="23c86-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23c86-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="23c86-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="23c86-417">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="23c86-417">**IIS Express**</span></span>

   * <span data-ttu-id="23c86-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="23c86-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="23c86-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="23c86-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="23c86-420">Configuración</span><span class="sxs-lookup"><span data-stu-id="23c86-420">Configuration</span></span>

<span data-ttu-id="23c86-421">**IIS**</span><span class="sxs-lookup"><span data-stu-id="23c86-421">**IIS**</span></span>

   * <span data-ttu-id="23c86-422">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="23c86-422">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="23c86-423">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="23c86-423">**IIS Express**</span></span>

   * <span data-ttu-id="23c86-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="23c86-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="23c86-425">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="23c86-425">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
