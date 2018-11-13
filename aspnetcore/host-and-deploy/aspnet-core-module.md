---
title: Referencia de configuración del módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ca86b1548c7c28a64fd391617b2e8290c1c264cf
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191365"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="06e9a-103">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06e9a-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="06e9a-104">Por [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="06e9a-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="06e9a-105">En este documento se proporcionan instrucciones sobre cómo configurar el módulo ASP.NET Core para hospedar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="06e9a-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="06e9a-106">Puede encontrar una introducción al módulo ASP.NET Core e instrucciones de instalación en el artículo de [introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="06e9a-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="06e9a-107">Modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="06e9a-107">Hosting model</span></span>

<span data-ttu-id="06e9a-108">En las aplicaciones con .NET Core 2.2 o posterior, el módulo admite un modelo de hospedaje en proceso para un mejor rendimiento en comparación con el hospedaje de proxy inverso (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="06e9a-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="06e9a-109">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="06e9a-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="06e9a-110">El hospedaje en proceso es opcional para las aplicaciones existentes pero, para las plantillas [dotnet new](/dotnet/core/tools/dotnet-new), este modelo es el predeterminado para todos los escenarios de IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="06e9a-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="06e9a-111">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación (por ejemplo, *MiAplicación.csproj*) con el valor `inprocess` (el hospedaje fuera de proceso se establece con `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="06e9a-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="06e9a-112">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="06e9a-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="06e9a-113">No se usa el [servidor de Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="06e9a-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="06e9a-114">Una implementación <xref:Microsoft.AspNetCore.Hosting.Server.IServer> personalizada, `IISHttpServer` actúa como servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="06e9a-115">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="06e9a-116">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="06e9a-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="06e9a-117">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-117">Use one app pool per app.</span></span>

* <span data-ttu-id="06e9a-118">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="06e9a-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="06e9a-119">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="06e9a-120">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="06e9a-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="06e9a-121">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="06e9a-122">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="06e9a-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="06e9a-123">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="06e9a-123">Client disconnects are detected.</span></span> <span data-ttu-id="06e9a-124">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="06e9a-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="06e9a-125">`Directory.GetCurrentDirectory()` devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del directorio de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="06e9a-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="06e9a-126">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="06e9a-126">Hosting model changes</span></span>

<span data-ttu-id="06e9a-127">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="06e9a-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="06e9a-128">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="06e9a-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="06e9a-129">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="06e9a-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="06e9a-130">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="06e9a-130">Process name</span></span>

<span data-ttu-id="06e9a-131">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="06e9a-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="06e9a-132">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="06e9a-132">Configuration with web.config</span></span>

<span data-ttu-id="06e9a-133">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="06e9a-134">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="06e9a-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
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

<span data-ttu-id="06e9a-135">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="06e9a-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
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
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="06e9a-136">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-136">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="06e9a-137">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="06e9a-137">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="06e9a-138">Consulte [Configuración de aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-application-configuration) para ver una nota importante relativa a la configuración de archivos *web.config* en aplicaciones secundarias.</span><span class="sxs-lookup"><span data-stu-id="06e9a-138">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="06e9a-139">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="06e9a-139">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="06e9a-140">Atributo</span><span class="sxs-lookup"><span data-stu-id="06e9a-140">Attribute</span></span> | <span data-ttu-id="06e9a-141">Descripción</span><span class="sxs-lookup"><span data-stu-id="06e9a-141">Description</span></span> | <span data-ttu-id="06e9a-142">Default</span><span class="sxs-lookup"><span data-stu-id="06e9a-142">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="06e9a-143">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-143">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-144">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-144">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="06e9a-145">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-145">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-146">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-146">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="06e9a-147">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-148">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-148">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="06e9a-149">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-149">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="06e9a-150">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-150">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-151">Especifica el modelo de hospedaje como en proceso (`inprocess`) o fuera de proceso (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="06e9a-151">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="06e9a-152">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-152">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-153">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-153">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="06e9a-154">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-154">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="06e9a-155">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-155">Default: `1`</span></span><br><span data-ttu-id="06e9a-156">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-156">Min: `1`</span></span><br><span data-ttu-id="06e9a-157">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="06e9a-157">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="06e9a-158">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="06e9a-158">Required string attribute.</span></span></p><p><span data-ttu-id="06e9a-159">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="06e9a-159">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="06e9a-160">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-160">Relative paths are supported.</span></span> <span data-ttu-id="06e9a-161">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-161">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="06e9a-162">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-162">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-163">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-163">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="06e9a-164">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-164">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="06e9a-165">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-165">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="06e9a-166">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-166">Default: `10`</span></span><br><span data-ttu-id="06e9a-167">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-167">Min: `0`</span></span><br><span data-ttu-id="06e9a-168">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="06e9a-168">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="06e9a-169">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-169">Optional timespan attribute.</span></span></p><p><span data-ttu-id="06e9a-170">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="06e9a-170">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="06e9a-171">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="06e9a-171">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="06e9a-172">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-172">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="06e9a-173">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-173">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="06e9a-174">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-174">Default: `00:02:00`</span></span><br><span data-ttu-id="06e9a-175">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-175">Min: `00:00:00`</span></span><br><span data-ttu-id="06e9a-176">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-176">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="06e9a-177">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-177">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-178">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-178">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="06e9a-179">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-179">Default: `10`</span></span><br><span data-ttu-id="06e9a-180">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-180">Min: `0`</span></span><br><span data-ttu-id="06e9a-181">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-181">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="06e9a-182">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-183">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-183">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="06e9a-184">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-184">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="06e9a-185">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="06e9a-185">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="06e9a-186">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="06e9a-186">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="06e9a-187">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="06e9a-187">Default: `120`</span></span><br><span data-ttu-id="06e9a-188">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-188">Min: `0`</span></span><br><span data-ttu-id="06e9a-189">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-189">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="06e9a-190">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-191">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-191">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="06e9a-192">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-192">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-193">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-193">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="06e9a-194">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-194">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="06e9a-195">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-195">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="06e9a-196">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="06e9a-196">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="06e9a-197">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-197">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="06e9a-198">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="06e9a-198">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="06e9a-199">Atributo</span><span class="sxs-lookup"><span data-stu-id="06e9a-199">Attribute</span></span> | <span data-ttu-id="06e9a-200">Descripción</span><span class="sxs-lookup"><span data-stu-id="06e9a-200">Description</span></span> | <span data-ttu-id="06e9a-201">Default</span><span class="sxs-lookup"><span data-stu-id="06e9a-201">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="06e9a-202">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-202">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-203">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-203">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="06e9a-204">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-204">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-205">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-205">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="06e9a-206">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-206">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-207">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-207">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="06e9a-208">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-208">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="06e9a-209">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-209">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-210">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-210">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="06e9a-211">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-211">Default: `1`</span></span><br><span data-ttu-id="06e9a-212">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-212">Min: `1`</span></span><br><span data-ttu-id="06e9a-213">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="06e9a-213">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="06e9a-214">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="06e9a-214">Required string attribute.</span></span></p><p><span data-ttu-id="06e9a-215">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="06e9a-215">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="06e9a-216">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-216">Relative paths are supported.</span></span> <span data-ttu-id="06e9a-217">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-217">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="06e9a-218">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-218">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-219">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-219">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="06e9a-220">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-220">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="06e9a-221">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-221">Default: `10`</span></span><br><span data-ttu-id="06e9a-222">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-222">Min: `0`</span></span><br><span data-ttu-id="06e9a-223">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="06e9a-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="06e9a-224">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="06e9a-225">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="06e9a-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="06e9a-226">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="06e9a-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="06e9a-227">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-227">Default: `00:02:00`</span></span><br><span data-ttu-id="06e9a-228">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-228">Min: `00:00:00`</span></span><br><span data-ttu-id="06e9a-229">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-229">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="06e9a-230">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-230">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-231">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-231">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="06e9a-232">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-232">Default: `10`</span></span><br><span data-ttu-id="06e9a-233">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-233">Min: `0`</span></span><br><span data-ttu-id="06e9a-234">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-234">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="06e9a-235">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-235">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-236">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-236">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="06e9a-237">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-237">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="06e9a-238">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="06e9a-238">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="06e9a-239">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="06e9a-239">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="06e9a-240">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="06e9a-240">Default: `120`</span></span><br><span data-ttu-id="06e9a-241">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-241">Min: `0`</span></span><br><span data-ttu-id="06e9a-242">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-242">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="06e9a-243">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-243">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-244">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-244">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="06e9a-245">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-245">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-246">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-246">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="06e9a-247">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-247">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="06e9a-248">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-248">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="06e9a-249">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="06e9a-249">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="06e9a-250">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-250">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="06e9a-251">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="06e9a-251">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="06e9a-252">Atributo</span><span class="sxs-lookup"><span data-stu-id="06e9a-252">Attribute</span></span> | <span data-ttu-id="06e9a-253">Descripción</span><span class="sxs-lookup"><span data-stu-id="06e9a-253">Description</span></span> | <span data-ttu-id="06e9a-254">Default</span><span class="sxs-lookup"><span data-stu-id="06e9a-254">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="06e9a-255">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-255">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-256">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-256">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="06e9a-257">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-257">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-258">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-258">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="06e9a-259">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-260">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-260">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="06e9a-261">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="06e9a-261">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="06e9a-262">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-263">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-263">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="06e9a-264">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-264">Default: `1`</span></span><br><span data-ttu-id="06e9a-265">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="06e9a-265">Min: `1`</span></span><br><span data-ttu-id="06e9a-266">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="06e9a-266">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="06e9a-267">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="06e9a-267">Required string attribute.</span></span></p><p><span data-ttu-id="06e9a-268">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="06e9a-268">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="06e9a-269">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-269">Relative paths are supported.</span></span> <span data-ttu-id="06e9a-270">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-270">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="06e9a-271">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-272">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-272">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="06e9a-273">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-273">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="06e9a-274">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-274">Default: `10`</span></span><br><span data-ttu-id="06e9a-275">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-275">Min: `0`</span></span><br><span data-ttu-id="06e9a-276">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="06e9a-276">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="06e9a-277">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-277">Optional timespan attribute.</span></span></p><p><span data-ttu-id="06e9a-278">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="06e9a-278">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="06e9a-279">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="06e9a-279">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="06e9a-280">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-280">Default: `00:02:00`</span></span><br><span data-ttu-id="06e9a-281">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-281">Min: `00:00:00`</span></span><br><span data-ttu-id="06e9a-282">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="06e9a-282">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="06e9a-283">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-283">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-284">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-284">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="06e9a-285">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="06e9a-285">Default: `10`</span></span><br><span data-ttu-id="06e9a-286">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-286">Min: `0`</span></span><br><span data-ttu-id="06e9a-287">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-287">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="06e9a-288">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-288">Optional integer attribute.</span></span></p><p><span data-ttu-id="06e9a-289">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="06e9a-289">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="06e9a-290">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-290">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="06e9a-291">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="06e9a-291">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="06e9a-292">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="06e9a-292">Default: `120`</span></span><br><span data-ttu-id="06e9a-293">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="06e9a-293">Min: `0`</span></span><br><span data-ttu-id="06e9a-294">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="06e9a-294">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="06e9a-295">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-295">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="06e9a-296">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-296">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="06e9a-297">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="06e9a-297">Optional string attribute.</span></span></p><p><span data-ttu-id="06e9a-298">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-298">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="06e9a-299">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="06e9a-299">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="06e9a-300">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="06e9a-300">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="06e9a-301">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="06e9a-301">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="06e9a-302">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-302">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="06e9a-303">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="06e9a-303">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="06e9a-304">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="06e9a-304">Setting environment variables</span></span>

<span data-ttu-id="06e9a-305">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-305">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="06e9a-306">Especifique una variable de entorno con el elemento secundario `environmentVariable` de un elemento de la colección `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-306">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="06e9a-307">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="06e9a-307">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="06e9a-308">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="06e9a-308">The following example sets two environment variables.</span></span> <span data-ttu-id="06e9a-309">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-309">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="06e9a-310">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-310">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="06e9a-311">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-311">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
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
> <span data-ttu-id="06e9a-312">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="06e9a-312">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="06e9a-313">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="06e9a-313">app_offline.htm</span></span>

<span data-ttu-id="06e9a-314">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="06e9a-314">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="06e9a-315">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="06e9a-315">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="06e9a-316">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-316">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="06e9a-317">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-317">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e9a-318">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="06e9a-318">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="06e9a-319">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-319">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="06e9a-320">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="06e9a-320">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e9a-321">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-321">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="06e9a-322">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-322">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="06e9a-323">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-323">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="06e9a-324">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-324">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="06e9a-325">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-325">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="06e9a-326">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="06e9a-326">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="06e9a-327">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-327">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="06e9a-328">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-328">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="06e9a-329">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="06e9a-329">For more information on configuring custom error messages, see [HTTP Errors &lt;httpErrors&gt;](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="06e9a-331">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="06e9a-331">Log creation and redirection</span></span>

<span data-ttu-id="06e9a-332">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="06e9a-332">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="06e9a-333">Las carpetas de la ruta de acceso `stdoutLogFile` debe estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="06e9a-333">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="06e9a-334">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="06e9a-334">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="06e9a-335">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="06e9a-335">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="06e9a-336">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="06e9a-336">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="06e9a-337">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-337">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="06e9a-338">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="06e9a-338">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="06e9a-339">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="06e9a-339">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="06e9a-340">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="06e9a-340">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="06e9a-341">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="06e9a-341">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="06e9a-342">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="06e9a-342">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="06e9a-343">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-343">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e9a-344">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="06e9a-344">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="06e9a-345">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="06e9a-345">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="06e9a-346">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="06e9a-346">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="06e9a-347">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="06e9a-347">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="06e9a-348">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="06e9a-348">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="06e9a-349">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="06e9a-349">Enhanced diagnostic logs</span></span>

<span data-ttu-id="06e9a-350">El módulo ASP.NET Core que se proporciona es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="06e9a-350">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="06e9a-351">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="06e9a-351">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="06e9a-352">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-352">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="06e9a-353">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="06e9a-353">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="06e9a-354">ERROR</span><span class="sxs-lookup"><span data-stu-id="06e9a-354">ERROR</span></span>
* <span data-ttu-id="06e9a-355">WARNING</span><span class="sxs-lookup"><span data-stu-id="06e9a-355">WARNING</span></span>
* <span data-ttu-id="06e9a-356">INFO</span><span class="sxs-lookup"><span data-stu-id="06e9a-356">INFO</span></span>
* <span data-ttu-id="06e9a-357">TRACE</span><span class="sxs-lookup"><span data-stu-id="06e9a-357">TRACE</span></span>

<span data-ttu-id="06e9a-358">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="06e9a-358">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="06e9a-359">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="06e9a-359">CONSOLE</span></span>
* <span data-ttu-id="06e9a-360">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="06e9a-360">EVENTLOG</span></span>
* <span data-ttu-id="06e9a-361">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="06e9a-361">FILE</span></span>

<span data-ttu-id="06e9a-362">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="06e9a-362">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="06e9a-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="06e9a-363">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="06e9a-364">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="06e9a-364">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="06e9a-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="06e9a-365">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="06e9a-366">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="06e9a-366">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="06e9a-367">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="06e9a-367">The size of the log isn't limited.</span></span> <span data-ttu-id="06e9a-368">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="06e9a-368">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="06e9a-369">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-369">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="06e9a-370">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="06e9a-370">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="06e9a-371">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="06e9a-371">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="06e9a-372">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="06e9a-372">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="06e9a-373">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="06e9a-373">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="06e9a-374">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="06e9a-374">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="06e9a-375">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="06e9a-375">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="06e9a-376">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="06e9a-376">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="06e9a-377">El token de emparejamiento también se establece en un encabezado (`MSAspNetCoreToken`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="06e9a-377">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="06e9a-378">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="06e9a-378">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="06e9a-379">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="06e9a-379">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="06e9a-380">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="06e9a-380">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="06e9a-381">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="06e9a-381">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="06e9a-382">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="06e9a-382">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="06e9a-383">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="06e9a-383">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="06e9a-384">Dado que la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador recibe un error de acceso denegado al intentar configurar los valores del módulo en *applicationHost.config* en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="06e9a-384">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="06e9a-385">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="06e9a-385">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="06e9a-386">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="06e9a-386">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="06e9a-387">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="06e9a-387">Run the installer.</span></span>
1. <span data-ttu-id="06e9a-388">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="06e9a-388">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="06e9a-389">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="06e9a-389">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="06e9a-390">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="06e9a-390">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="06e9a-391">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="06e9a-391">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="06e9a-392">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-392">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="06e9a-393">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-393">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="06e9a-394">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="06e9a-394">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="06e9a-395">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="06e9a-395">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="06e9a-396">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-396">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="06e9a-397">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="06e9a-397">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="06e9a-398">Module</span><span class="sxs-lookup"><span data-stu-id="06e9a-398">Module</span></span>

<span data-ttu-id="06e9a-399">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="06e9a-399">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="06e9a-400">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-400">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="06e9a-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-401">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="06e9a-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-402">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="06e9a-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-403">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="06e9a-404">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="06e9a-404">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="06e9a-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-405">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="06e9a-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-406">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="06e9a-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-407">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="06e9a-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="06e9a-408">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="06e9a-409">Schema</span><span class="sxs-lookup"><span data-stu-id="06e9a-409">Schema</span></span>

<span data-ttu-id="06e9a-410">**IIS**</span><span class="sxs-lookup"><span data-stu-id="06e9a-410">**IIS**</span></span>

   * <span data-ttu-id="06e9a-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="06e9a-411">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="06e9a-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="06e9a-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="06e9a-413">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="06e9a-413">**IIS Express**</span></span>

   * <span data-ttu-id="06e9a-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="06e9a-414">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="06e9a-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="06e9a-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="06e9a-416">Configuración</span><span class="sxs-lookup"><span data-stu-id="06e9a-416">Configuration</span></span>

<span data-ttu-id="06e9a-417">**IIS**</span><span class="sxs-lookup"><span data-stu-id="06e9a-417">**IIS**</span></span>

   * <span data-ttu-id="06e9a-418">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="06e9a-418">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="06e9a-419">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="06e9a-419">**IIS Express**</span></span>

   * <span data-ttu-id="06e9a-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="06e9a-420">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="06e9a-421">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="06e9a-421">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>