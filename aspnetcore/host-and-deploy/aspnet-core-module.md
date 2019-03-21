---
title: Módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ead3732d2c272f0868e823726ffd415a6c74f444
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265462"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="b5fe0-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5fe0-103">ASP.NET Core Module</span></span>

<span data-ttu-id="b5fe0-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b5fe0-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-105">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="b5fe0-106">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="b5fe0-107">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="b5fe0-108">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-108">Supported Windows versions:</span></span>

* <span data-ttu-id="b5fe0-109">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="b5fe0-109">Windows 7 or later</span></span>
* <span data-ttu-id="b5fe0-110">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="b5fe0-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="b5fe0-111">En el caso del hospedaje en proceso, el módulo usa el servidor HTTP de IIS (`IISHttpServer`), una implementación de servidor en proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="b5fe0-112">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="b5fe0-113">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="b5fe0-114">Modelos de hospedaje</span><span class="sxs-lookup"><span data-stu-id="b5fe0-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="b5fe0-115">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="b5fe0-115">In-process hosting model</span></span>

<span data-ttu-id="b5fe0-116">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación con un valor de `InProcess` (el hospedaje fuera de proceso se establece con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="b5fe0-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="b5fe0-117">No se admite el modelo de hospedaje en proceso para aplicaciones de ASP.NET Core que tienen como destino .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="b5fe0-118">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="b5fe0-119">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="b5fe0-120">En lugar del servidor HTTP de IIS (`IISHttpServer`) se usa el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="b5fe0-121">En el mismo proceso, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> para:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="b5fe0-122">Registrar el `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="b5fe0-123">Configurar el puerto y la ruta de acceso base donde debe escuchar el servidor al ejecutarse detrás del módulo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="b5fe0-124">Configurar el host para capturar errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="b5fe0-125">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="b5fe0-126">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="b5fe0-127">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-127">Use one app pool per app.</span></span>

* <span data-ttu-id="b5fe0-128">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b5fe0-129">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="b5fe0-130">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="b5fe0-131">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="b5fe0-132">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="b5fe0-133">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-133">Client disconnects are detected.</span></span> <span data-ttu-id="b5fe0-134">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="b5fe0-135">En ASP.NET Core 2.2.1 o versiones anteriores, <xref:System.IO.Directory.GetCurrentDirectory*> devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="b5fe0-136">Para conocer el código de ejemplo que establece el directorio actual de la aplicación, consulte la información sobre la [clase CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="b5fe0-137">Llame al método `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="b5fe0-138">Las llamadas subsiguientes a <xref:System.IO.Directory.GetCurrentDirectory*> proporcionan el directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="b5fe0-139">Cuando se hospeda en el proceso, no se llama a <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> de forma interna para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="b5fe0-140">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="b5fe0-141">Al transformar notificaciones con una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, llame a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> para agregar servicios de autenticación:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="b5fe0-142">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="b5fe0-142">Out-of-process hosting model</span></span>

<span data-ttu-id="b5fe0-143">Para configurar una aplicación para el hospedaje fuera de proceso, use uno de los métodos siguientes en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="b5fe0-144">No especifique la propiedad `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="b5fe0-145">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="b5fe0-146">Establezca el valor de la propiedad `<AspNetCoreHostingModel>` en `OutOfProcess` (el hospedaje en proceso se establece con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="b5fe0-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="b5fe0-147">En lugar del servidor [Kestrel](xref:fundamentals/servers/kestrel) se usa el servidor HTTP de IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="b5fe0-148">Fuera del proceso, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="b5fe0-149">Configurar el puerto y la ruta de acceso base donde debe escuchar el servidor al ejecutarse detrás del módulo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="b5fe0-150">Configurar el host para capturar errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="b5fe0-151">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="b5fe0-151">Hosting model changes</span></span>

<span data-ttu-id="b5fe0-152">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="b5fe0-153">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="b5fe0-154">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="b5fe0-155">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="b5fe0-155">Process name</span></span>

<span data-ttu-id="b5fe0-156">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b5fe0-157">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="b5fe0-158">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-158">Supported Windows versions:</span></span>

* <span data-ttu-id="b5fe0-159">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="b5fe0-159">Windows 7 or later</span></span>
* <span data-ttu-id="b5fe0-160">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="b5fe0-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="b5fe0-161">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-161">The module only works with Kestrel.</span></span> <span data-ttu-id="b5fe0-162">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="b5fe0-163">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="b5fe0-164">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="b5fe0-165">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="b5fe0-166">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="b5fe0-168">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="b5fe0-169">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="b5fe0-170">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="b5fe0-171">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-171">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="b5fe0-172">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="b5fe0-173">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="b5fe0-174">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="b5fe0-175">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="b5fe0-176">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="b5fe0-177">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-178">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="b5fe0-179">Para obtener más información sobre los módulos de IIS activos con el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="b5fe0-180">El módulo ASP.NET Core también puede:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="b5fe0-181">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="b5fe0-182">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="b5fe0-183">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="b5fe0-184">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5fe0-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="b5fe0-185">Para obtener instrucciones sobre cómo instalar y usar el módulo ASP.NET Core, consulte <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="b5fe0-186">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="b5fe0-186">Configuration with web.config</span></span>

<span data-ttu-id="b5fe0-187">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="b5fe0-188">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="b5fe0-189">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="b5fe0-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="b5fe0-190">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="b5fe0-191">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="b5fe0-192">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="b5fe0-193">Para obtener información sobre la configuración de aplicaciones secundarias de IIS, consulte <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="b5fe0-194">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="b5fe0-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="b5fe0-195">Atributo</span><span class="sxs-lookup"><span data-stu-id="b5fe0-195">Attribute</span></span> | <span data-ttu-id="b5fe0-196">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5fe0-196">Description</span></span> | <span data-ttu-id="b5fe0-197">Default</span><span class="sxs-lookup"><span data-stu-id="b5fe0-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b5fe0-198">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-198">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-199">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b5fe0-200">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-201">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b5fe0-202">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-203">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b5fe0-204">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="b5fe0-205">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-205">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-206">Especifica el modelo de hospedaje como en proceso (`InProcess`) o fuera de proceso (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="b5fe0-207">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-208">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="b5fe0-209">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="b5fe0-210">No se recomienda establecer `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-210">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="b5fe0-211">Este atributo se quitará en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-211">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="b5fe0-212">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-212">Default: `1`</span></span><br><span data-ttu-id="b5fe0-213">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-213">Min: `1`</span></span><br><span data-ttu-id="b5fe0-214">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="b5fe0-214">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="b5fe0-215">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-215">Required string attribute.</span></span></p><p><span data-ttu-id="b5fe0-216">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b5fe0-217">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-217">Relative paths are supported.</span></span> <span data-ttu-id="b5fe0-218">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b5fe0-219">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-220">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b5fe0-221">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="b5fe0-222">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-222">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="b5fe0-223">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-223">Default: `10`</span></span><br><span data-ttu-id="b5fe0-224">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-224">Min: `0`</span></span><br><span data-ttu-id="b5fe0-225">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-225">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b5fe0-226">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-226">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b5fe0-227">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-227">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b5fe0-228">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-228">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="b5fe0-229">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-229">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="b5fe0-230">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-230">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="b5fe0-231">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-231">Default: `00:02:00`</span></span><br><span data-ttu-id="b5fe0-232">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-232">Min: `00:00:00`</span></span><br><span data-ttu-id="b5fe0-233">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b5fe0-234">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-235">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b5fe0-236">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-236">Default: `10`</span></span><br><span data-ttu-id="b5fe0-237">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-237">Min: `0`</span></span><br><span data-ttu-id="b5fe0-238">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b5fe0-239">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-240">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b5fe0-241">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b5fe0-242">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b5fe0-243">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b5fe0-244">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-244">Default: `120`</span></span><br><span data-ttu-id="b5fe0-245">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-245">Min: `0`</span></span><br><span data-ttu-id="b5fe0-246">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b5fe0-247">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-248">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b5fe0-249">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-249">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-250">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b5fe0-251">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b5fe0-252">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b5fe0-253">Al crearse el archivo de registro, el módulo crea las carpetas que se proporcionan en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-253">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="b5fe0-254">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b5fe0-255">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b5fe0-256">Atributo</span><span class="sxs-lookup"><span data-stu-id="b5fe0-256">Attribute</span></span> | <span data-ttu-id="b5fe0-257">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5fe0-257">Description</span></span> | <span data-ttu-id="b5fe0-258">Default</span><span class="sxs-lookup"><span data-stu-id="b5fe0-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b5fe0-259">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-259">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-260">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b5fe0-261">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-262">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b5fe0-263">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-264">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b5fe0-265">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b5fe0-266">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-267">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="b5fe0-268">No se recomienda establecer `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-268">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="b5fe0-269">Este atributo se quitará en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-269">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="b5fe0-270">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-270">Default: `1`</span></span><br><span data-ttu-id="b5fe0-271">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-271">Min: `1`</span></span><br><span data-ttu-id="b5fe0-272">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-272">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b5fe0-273">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-273">Required string attribute.</span></span></p><p><span data-ttu-id="b5fe0-274">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-274">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b5fe0-275">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-275">Relative paths are supported.</span></span> <span data-ttu-id="b5fe0-276">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-276">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b5fe0-277">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-277">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-278">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-278">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b5fe0-279">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-279">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b5fe0-280">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-280">Default: `10`</span></span><br><span data-ttu-id="b5fe0-281">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-281">Min: `0`</span></span><br><span data-ttu-id="b5fe0-282">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-282">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b5fe0-283">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-283">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b5fe0-284">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-284">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b5fe0-285">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-285">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="b5fe0-286">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-286">Default: `00:02:00`</span></span><br><span data-ttu-id="b5fe0-287">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-287">Min: `00:00:00`</span></span><br><span data-ttu-id="b5fe0-288">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-288">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b5fe0-289">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-290">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-290">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b5fe0-291">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-291">Default: `10`</span></span><br><span data-ttu-id="b5fe0-292">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-292">Min: `0`</span></span><br><span data-ttu-id="b5fe0-293">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-293">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b5fe0-294">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-295">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-295">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b5fe0-296">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-296">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b5fe0-297">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-297">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="b5fe0-298">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-298">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="b5fe0-299">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-299">Default: `120`</span></span><br><span data-ttu-id="b5fe0-300">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-300">Min: `0`</span></span><br><span data-ttu-id="b5fe0-301">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-301">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b5fe0-302">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-303">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-303">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b5fe0-304">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-304">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-305">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-305">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b5fe0-306">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-306">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b5fe0-307">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-307">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b5fe0-308">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-308">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b5fe0-309">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-309">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b5fe0-310">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-310">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="b5fe0-311">Atributo</span><span class="sxs-lookup"><span data-stu-id="b5fe0-311">Attribute</span></span> | <span data-ttu-id="b5fe0-312">Descripción</span><span class="sxs-lookup"><span data-stu-id="b5fe0-312">Description</span></span> | <span data-ttu-id="b5fe0-313">Default</span><span class="sxs-lookup"><span data-stu-id="b5fe0-313">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="b5fe0-314">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-314">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-315">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-315">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="b5fe0-316">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-316">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-317">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-317">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="b5fe0-318">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-318">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-319">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-319">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="b5fe0-320">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-320">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="b5fe0-321">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-321">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-322">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-322">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="b5fe0-323">No se recomienda establecer `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-323">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="b5fe0-324">Este atributo se quitará en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-324">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="b5fe0-325">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-325">Default: `1`</span></span><br><span data-ttu-id="b5fe0-326">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-326">Min: `1`</span></span><br><span data-ttu-id="b5fe0-327">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-327">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="b5fe0-328">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-328">Required string attribute.</span></span></p><p><span data-ttu-id="b5fe0-329">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-329">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="b5fe0-330">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-330">Relative paths are supported.</span></span> <span data-ttu-id="b5fe0-331">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-331">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="b5fe0-332">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-333">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-333">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="b5fe0-334">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-334">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="b5fe0-335">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-335">Default: `10`</span></span><br><span data-ttu-id="b5fe0-336">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-336">Min: `0`</span></span><br><span data-ttu-id="b5fe0-337">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-337">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="b5fe0-338">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-338">Optional timespan attribute.</span></span></p><p><span data-ttu-id="b5fe0-339">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-339">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="b5fe0-340">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-340">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="b5fe0-341">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-341">Default: `00:02:00`</span></span><br><span data-ttu-id="b5fe0-342">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-342">Min: `00:00:00`</span></span><br><span data-ttu-id="b5fe0-343">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-343">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="b5fe0-344">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-344">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-345">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-345">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="b5fe0-346">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-346">Default: `10`</span></span><br><span data-ttu-id="b5fe0-347">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-347">Min: `0`</span></span><br><span data-ttu-id="b5fe0-348">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-348">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="b5fe0-349">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-349">Optional integer attribute.</span></span></p><p><span data-ttu-id="b5fe0-350">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-350">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="b5fe0-351">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-351">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="b5fe0-352">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-352">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="b5fe0-353">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-353">Default: `120`</span></span><br><span data-ttu-id="b5fe0-354">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-354">Min: `0`</span></span><br><span data-ttu-id="b5fe0-355">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="b5fe0-355">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="b5fe0-356">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-356">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="b5fe0-357">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-357">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="b5fe0-358">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-358">Optional string attribute.</span></span></p><p><span data-ttu-id="b5fe0-359">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-359">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="b5fe0-360">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-360">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="b5fe0-361">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-361">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="b5fe0-362">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-362">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="b5fe0-363">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-363">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="b5fe0-364">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-364">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="b5fe0-365">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b5fe0-365">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b5fe0-366">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-366">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="b5fe0-367">Especifique una variable de entorno con el elemento secundario `<environmentVariable>` de un elemento de la colección `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-367">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="b5fe0-368">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-368">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b5fe0-369">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-369">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="b5fe0-370">Especifique una variable de entorno con el elemento secundario `<environmentVariable>` de un elemento de la colección `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-370">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="b5fe0-371">Las variables de entorno establecidas en esta sección entran en conflicto con las variables de entorno del sistema que tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-371">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="b5fe0-372">Si se establece una variable de entorno en el archivo *web.config* y en el nivel del sistema en Windows, el valor del archivo *web.config* se anexa al valor de la variable de entorno del sistema (por ejemplo, `ASPNETCORE_ENVIRONMENT: Development;Development`), que impide que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-372">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-373">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-373">The following example sets two environment variables.</span></span> <span data-ttu-id="b5fe0-374">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-374">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="b5fe0-375">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-375">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="b5fe0-376">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-376">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="b5fe0-377">Una alternativa a establecer directamente el entorno en *web.config* consiste en incluir la propiedad `<EnvironmentName>` en el perfil de publicación (*.pubxml*) o el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-377">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="b5fe0-378">Este método establece el entorno en *web.config* cuando se publica el proyecto:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-378">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="b5fe0-379">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-379">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="b5fe0-380">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="b5fe0-380">app_offline.htm</span></span>

<span data-ttu-id="b5fe0-381">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-381">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="b5fe0-382">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-382">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="b5fe0-383">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-383">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="b5fe0-384">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-384">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-385">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-385">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="b5fe0-386">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-386">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="b5fe0-387">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="b5fe0-387">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-388">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-388">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="b5fe0-389">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-389">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="b5fe0-390">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-390">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="b5fe0-391">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-391">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="b5fe0-392">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-392">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="b5fe0-393">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-393">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b5fe0-394">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-394">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="b5fe0-395">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-395">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="b5fe0-396">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-396">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="b5fe0-398">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="b5fe0-398">Log creation and redirection</span></span>

<span data-ttu-id="b5fe0-399">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-399">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="b5fe0-400">Al crearse el archivo de registro, el módulo crea las carpetas de la ruta de acceso de `stdoutLogFile`.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-400">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="b5fe0-401">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-401">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="b5fe0-402">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-402">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="b5fe0-403">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-403">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="b5fe0-404">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-404">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="b5fe0-405">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-405">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="b5fe0-406">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-406">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b5fe0-407">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-407">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="b5fe0-408">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-408">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="b5fe0-409">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-409">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="b5fe0-410">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-410">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-411">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-411">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="b5fe0-412">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-412">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-413">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-413">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="b5fe0-414">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-414">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="b5fe0-415">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-415">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="b5fe0-416">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="b5fe0-416">Enhanced diagnostic logs</span></span>

<span data-ttu-id="b5fe0-417">El módulo ASP.NET Core es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-417">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="b5fe0-418">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-418">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="b5fe0-419">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-419">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="b5fe0-420">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="b5fe0-420">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="b5fe0-421">ERROR</span><span class="sxs-lookup"><span data-stu-id="b5fe0-421">ERROR</span></span>
* <span data-ttu-id="b5fe0-422">WARNING</span><span class="sxs-lookup"><span data-stu-id="b5fe0-422">WARNING</span></span>
* <span data-ttu-id="b5fe0-423">INFO</span><span class="sxs-lookup"><span data-stu-id="b5fe0-423">INFO</span></span>
* <span data-ttu-id="b5fe0-424">TRACE</span><span class="sxs-lookup"><span data-stu-id="b5fe0-424">TRACE</span></span>

<span data-ttu-id="b5fe0-425">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="b5fe0-425">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="b5fe0-426">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="b5fe0-426">CONSOLE</span></span>
* <span data-ttu-id="b5fe0-427">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="b5fe0-427">EVENTLOG</span></span>
* <span data-ttu-id="b5fe0-428">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="b5fe0-428">FILE</span></span>

<span data-ttu-id="b5fe0-429">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-429">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="b5fe0-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-430">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="b5fe0-431">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="b5fe0-431">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="b5fe0-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-432">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="b5fe0-433">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-433">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="b5fe0-434">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-434">The size of the log isn't limited.</span></span> <span data-ttu-id="b5fe0-435">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-435">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-436">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-436">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="b5fe0-437">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="b5fe0-437">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-438">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="b5fe0-438">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-439">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-439">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="b5fe0-440">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-440">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="b5fe0-441">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-441">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="b5fe0-442">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-442">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="b5fe0-443">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-443">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="b5fe0-444">El token de emparejamiento también se establece en un encabezado (`MS-ASPNETCORE-TOKEN`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-444">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="b5fe0-445">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-445">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="b5fe0-446">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-446">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="b5fe0-447">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-447">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="b5fe0-448">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-448">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="b5fe0-449">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="b5fe0-449">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="b5fe0-450">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-450">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="b5fe0-451">Como la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador inicia un error de acceso denegado al intentar configurar los valores del módulo en el archivo *applicationHost.config* del recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-451">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b5fe0-452">Cuando se usa una configuración compartida de IIS en el mismo equipo que la instalación de IIS, ejecute el instalador del lote de hospedaje de ASP.NET Core con el parámetro `OPT_NO_SHARED_CONFIG_CHECK` establecido en `1`:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-452">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="b5fe0-453">Cuando la ruta de acceso a la configuración compartida no se encuentra en el mismo equipo que la instalación de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-453">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="b5fe0-454">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-454">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="b5fe0-455">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-455">Run the installer.</span></span>
1. <span data-ttu-id="b5fe0-456">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-456">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="b5fe0-457">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-457">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b5fe0-458">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-458">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="b5fe0-459">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-459">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="b5fe0-460">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-460">Run the installer.</span></span>
1. <span data-ttu-id="b5fe0-461">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-461">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="b5fe0-462">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-462">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="b5fe0-463">Inicialización de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="b5fe0-463">Application Initialization</span></span>

<span data-ttu-id="b5fe0-464">[Inicialización de aplicaciones de IIS](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) es una característica de IIS que envía una solicitud HTTP a la aplicación al iniciarse o reciclarse el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-464">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="b5fe0-465">La solicitud desencadena el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-465">The request triggers the app to start.</span></span> <span data-ttu-id="b5fe0-466">Tanto el [modelo de hospedaje en proceso](xref:fundamentals/servers/index#in-process-hosting-model) como el [modelo de hospedaje fuera de proceso](xref:fundamentals/servers/index#out-of-process-hosting-model) pueden usar Inicialización de aplicaciones con la versión 2 del módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-466">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="b5fe0-467">Para habilitar Inicialización de aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-467">To enable Application Initialization:</span></span>

1. <span data-ttu-id="b5fe0-468">Confirme que la característica de rol Inicialización de aplicaciones de IIS está habilitada:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-468">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="b5fe0-469">En Windows 7 o posterior: Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-469">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="b5fe0-470">Abra **Internet Information Services** > **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-470">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="b5fe0-471">Active la casilla de **Inicialización de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-471">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="b5fe0-472">En Windows Server 2008 R2 o posterior, abra el **Asistente para agregar roles y características**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-472">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="b5fe0-473">Cuando tenga acceso al panel **Seleccionar servicios de rol**, abra el nodo **Desarrollo de aplicaciones** y active la casilla **Inicialización de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-473">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="b5fe0-474">En el Administrador de IIS, seleccione **Grupos de aplicaciones** en el panel **Conexiones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-474">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="b5fe0-475">Seleccione el grupo de aplicaciones de la aplicación en la lista.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-475">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="b5fe0-476">Seleccione **Configuración avanzada** en **Modificar grupo de aplicaciones**, en el panel **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-476">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b5fe0-477">Establezca **Modo de inicio** en **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-477">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="b5fe0-478">Abra el nodo **Sitios** en el panel **Conexiones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-478">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="b5fe0-479">Seleccione la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-479">Select the app.</span></span>
1. <span data-ttu-id="b5fe0-480">Seleccione **Configuración avanzada** en **Administrar sitio web**, en el panel **Acciones**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-480">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b5fe0-481">Establezca **Carga previa activada** en **True**.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-481">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="b5fe0-482">Para obtener más información, consulte [Inicialización de aplicaciones de IIS 8.0](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="b5fe0-482">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="b5fe0-483">Las aplicaciones que usan el [modelo de hospedaje fuera de proceso](xref:fundamentals/servers/index#out-of-process-hosting-model) deben utilizar un servicio externo para hacer ping a la aplicación de forma periódica a fin de mantenerla en ejecución.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-483">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="b5fe0-484">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="b5fe0-484">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="b5fe0-485">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="b5fe0-485">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="b5fe0-486">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-486">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="b5fe0-487">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-487">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="b5fe0-488">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-488">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="b5fe0-489">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-489">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="b5fe0-490">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-490">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="b5fe0-491">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="b5fe0-491">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="b5fe0-492">Module</span><span class="sxs-lookup"><span data-stu-id="b5fe0-492">Module</span></span>

<span data-ttu-id="b5fe0-493">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-493">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="b5fe0-494">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-494">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="b5fe0-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-495">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b5fe0-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-496">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="b5fe0-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-497">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-498">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-498">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="b5fe0-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-499">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="b5fe0-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-500">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b5fe0-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-501">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="b5fe0-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="b5fe0-502">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="b5fe0-503">Schema</span><span class="sxs-lookup"><span data-stu-id="b5fe0-503">Schema</span></span>

<span data-ttu-id="b5fe0-504">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-504">**IIS**</span></span>

* <span data-ttu-id="b5fe0-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b5fe0-505">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b5fe0-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="b5fe0-506">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="b5fe0-507">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-507">**IIS Express**</span></span>

* <span data-ttu-id="b5fe0-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="b5fe0-508">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b5fe0-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="b5fe0-509">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="b5fe0-510">Configuración</span><span class="sxs-lookup"><span data-stu-id="b5fe0-510">Configuration</span></span>

<span data-ttu-id="b5fe0-511">**IIS**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-511">**IIS**</span></span>

* <span data-ttu-id="b5fe0-512">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b5fe0-512">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="b5fe0-513">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="b5fe0-513">**IIS Express**</span></span>

* <span data-ttu-id="b5fe0-514">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="b5fe0-514">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="b5fe0-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="b5fe0-515">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="b5fe0-516">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="b5fe0-516">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5fe0-517">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b5fe0-517">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="b5fe0-518">Repositorio GitHub del módulo ASP.NET Core (origen de referencia)</span><span class="sxs-lookup"><span data-stu-id="b5fe0-518">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
