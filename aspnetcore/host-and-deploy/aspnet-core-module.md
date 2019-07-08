---
title: Módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4a360023cc7fab2f066d490f7f368fc35815703a
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500443"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="5527b-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5527b-103">ASP.NET Core Module</span></span>

<span data-ttu-id="5527b-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5527b-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-105">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="5527b-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="5527b-106">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="5527b-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="5527b-107">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="5527b-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="5527b-108">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="5527b-108">Supported Windows versions:</span></span>

* <span data-ttu-id="5527b-109">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="5527b-109">Windows 7 or later</span></span>
* <span data-ttu-id="5527b-110">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="5527b-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="5527b-111">En el caso del hospedaje en proceso, el módulo usa el servidor HTTP de IIS (`IISHttpServer`), una implementación de servidor en proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="5527b-112">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5527b-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="5527b-113">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="5527b-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="5527b-114">Modelos de hospedaje</span><span class="sxs-lookup"><span data-stu-id="5527b-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="5527b-115">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="5527b-115">In-process hosting model</span></span>

<span data-ttu-id="5527b-116">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación con un valor de `InProcess` (el hospedaje fuera de proceso se establece con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="5527b-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="5527b-117">No se admite el modelo de hospedaje en proceso para aplicaciones de ASP.NET Core que tienen como destino .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5527b-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="5527b-118">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="5527b-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="5527b-119">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="5527b-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="5527b-120">En lugar del servidor HTTP de IIS (`IISHttpServer`) se usa el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5527b-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="5527b-121">En el mismo proceso, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> para:</span><span class="sxs-lookup"><span data-stu-id="5527b-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="5527b-122">Registrar el `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="5527b-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="5527b-123">Configurar el puerto y la ruta de acceso base donde debe escuchar el servidor al ejecutarse detrás del módulo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5527b-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="5527b-124">Configurar el host para capturar errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="5527b-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="5527b-125">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="5527b-126">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="5527b-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="5527b-127">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-127">Use one app pool per app.</span></span>

* <span data-ttu-id="5527b-128">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="5527b-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="5527b-129">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="5527b-130">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="5527b-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="5527b-131">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="5527b-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="5527b-132">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="5527b-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="5527b-133">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="5527b-133">Client disconnects are detected.</span></span> <span data-ttu-id="5527b-134">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="5527b-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="5527b-135">En ASP.NET Core 2.2.1 o versiones anteriores, <xref:System.IO.Directory.GetCurrentDirectory*> devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="5527b-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="5527b-136">Para conocer el código de ejemplo que establece el directorio actual de la aplicación, consulte la información sobre la [clase CurrentDirectoryHelpers](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="5527b-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="5527b-137">Llame al método `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="5527b-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="5527b-138">Las llamadas subsiguientes a <xref:System.IO.Directory.GetCurrentDirectory*> proporcionan el directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="5527b-139">Cuando se hospeda en el proceso, no se llama a <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> de forma interna para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="5527b-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="5527b-140">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5527b-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="5527b-141">Al transformar notificaciones con una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, llame a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> para agregar servicios de autenticación:</span><span class="sxs-lookup"><span data-stu-id="5527b-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="5527b-142">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="5527b-142">Out-of-process hosting model</span></span>

<span data-ttu-id="5527b-143">Para configurar una aplicación para el hospedaje fuera de proceso, use uno de los métodos siguientes en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="5527b-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="5527b-144">No especifique la propiedad `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="5527b-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="5527b-145">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="5527b-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="5527b-146">Establezca el valor de la propiedad `<AspNetCoreHostingModel>` en `OutOfProcess` (el hospedaje en proceso se establece con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="5527b-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="5527b-147">En lugar del servidor [Kestrel](xref:fundamentals/servers/kestrel) se usa el servidor HTTP de IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="5527b-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="5527b-148">Fuera del proceso, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para:</span><span class="sxs-lookup"><span data-stu-id="5527b-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="5527b-149">Configurar el puerto y la ruta de acceso base donde debe escuchar el servidor al ejecutarse detrás del módulo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5527b-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="5527b-150">Configurar el host para capturar errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="5527b-150">Configure the host to capture startup errors.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5527b-151">Cuando se hospeda fuera del proceso, no se llama a <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> de manera interna para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="5527b-151">When hosting out-of-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="5527b-152">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5527b-152">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="5527b-153">Al transformar notificaciones con una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, llame a <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> para agregar servicios de autenticación:</span><span class="sxs-lookup"><span data-stu-id="5527b-153">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
    services.AddAuthentication(IISDefaults.AuthenticationScheme);
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="hosting-model-changes"></a><span data-ttu-id="5527b-154">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="5527b-154">Hosting model changes</span></span>

<span data-ttu-id="5527b-155">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-155">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="5527b-156">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="5527b-156">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="5527b-157">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5527b-157">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="5527b-158">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="5527b-158">Process name</span></span>

<span data-ttu-id="5527b-159">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="5527b-159">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5527b-160">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="5527b-160">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="5527b-161">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="5527b-161">Supported Windows versions:</span></span>

* <span data-ttu-id="5527b-162">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="5527b-162">Windows 7 or later</span></span>
* <span data-ttu-id="5527b-163">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="5527b-163">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="5527b-164">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5527b-164">The module only works with Kestrel.</span></span> <span data-ttu-id="5527b-165">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="5527b-165">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="5527b-166">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="5527b-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="5527b-167">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="5527b-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="5527b-168">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="5527b-168">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="5527b-169">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="5527b-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="5527b-171">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="5527b-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="5527b-172">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5527b-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="5527b-173">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="5527b-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="5527b-174">El módulo especifica el puerto a través de la variable de entorno en el inicio, y el [middleware de integración de IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="5527b-174">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="5527b-175">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="5527b-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="5527b-176">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5527b-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="5527b-177">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5527b-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="5527b-178">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="5527b-179">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5527b-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="5527b-180">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="5527b-181">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="5527b-181">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="5527b-182">Para obtener más información sobre los módulos de IIS activos con el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="5527b-182">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="5527b-183">El módulo ASP.NET Core también puede:</span><span class="sxs-lookup"><span data-stu-id="5527b-183">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="5527b-184">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="5527b-184">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="5527b-185">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="5527b-185">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="5527b-186">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="5527b-186">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="5527b-187">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5527b-187">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="5527b-188">Para obtener instrucciones sobre cómo instalar y usar el módulo ASP.NET Core, consulte [Instalación del conjunto de hospedaje de .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="5527b-188">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="5527b-189">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="5527b-189">Configuration with web.config</span></span>

<span data-ttu-id="5527b-190">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="5527b-190">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="5527b-191">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="5527b-191">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="5527b-192">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="5527b-192">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="5527b-193">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="5527b-193">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="5527b-194">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="5527b-194">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="5527b-195">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5527b-195">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="5527b-196">Para obtener información sobre la configuración de aplicaciones secundarias de IIS, consulte <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="5527b-196">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="5527b-197">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="5527b-197">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="5527b-198">Atributo</span><span class="sxs-lookup"><span data-stu-id="5527b-198">Attribute</span></span> | <span data-ttu-id="5527b-199">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5527b-199">Description</span></span> | <span data-ttu-id="5527b-200">Default</span><span class="sxs-lookup"><span data-stu-id="5527b-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="5527b-201">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-201">Optional string attribute.</span></span></p><p><span data-ttu-id="5527b-202">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="5527b-202">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="5527b-203">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-204">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="5527b-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="5527b-205">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-206">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="5527b-207">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="5527b-208">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-208">Optional string attribute.</span></span></p><p><span data-ttu-id="5527b-209">Especifica el modelo de hospedaje como en proceso (`InProcess`) o fuera de proceso (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="5527b-209">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="5527b-210">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-211">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="5527b-212">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="5527b-212">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="5527b-213">No se recomienda establecer `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="5527b-213">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="5527b-214">Este atributo se quitará en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="5527b-214">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="5527b-215">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="5527b-215">Default: `1`</span></span><br><span data-ttu-id="5527b-216">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="5527b-216">Min: `1`</span></span><br><span data-ttu-id="5527b-217">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="5527b-217">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="5527b-218">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="5527b-218">Required string attribute.</span></span></p><p><span data-ttu-id="5527b-219">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5527b-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="5527b-220">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="5527b-220">Relative paths are supported.</span></span> <span data-ttu-id="5527b-221">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="5527b-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="5527b-222">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-223">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="5527b-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="5527b-224">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="5527b-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="5527b-225">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-225">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="5527b-226">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="5527b-226">Default: `10`</span></span><br><span data-ttu-id="5527b-227">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-227">Min: `0`</span></span><br><span data-ttu-id="5527b-228">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="5527b-228">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="5527b-229">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-229">Optional timespan attribute.</span></span></p><p><span data-ttu-id="5527b-230">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="5527b-230">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="5527b-231">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="5527b-231">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="5527b-232">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-232">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="5527b-233">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-233">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="5527b-234">Los valores válidos para los segmentos de minutos y segundos de la cadena se encuentran en el rango 0-59.</span><span class="sxs-lookup"><span data-stu-id="5527b-234">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="5527b-235">El uso de **60** en el valor de minutos o segundos da como resultado el error *500: Error interno del servidor*.</span><span class="sxs-lookup"><span data-stu-id="5527b-235">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="5527b-236">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-236">Default: `00:02:00`</span></span><br><span data-ttu-id="5527b-237">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-237">Min: `00:00:00`</span></span><br><span data-ttu-id="5527b-238">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-238">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="5527b-239">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-240">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="5527b-240">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="5527b-241">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="5527b-241">Default: `10`</span></span><br><span data-ttu-id="5527b-242">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-242">Min: `0`</span></span><br><span data-ttu-id="5527b-243">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="5527b-243">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="5527b-244">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-244">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-245">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="5527b-245">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="5527b-246">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-246">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="5527b-247">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="5527b-247">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="5527b-248">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="5527b-248">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="5527b-249">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="5527b-249">Default: `120`</span></span><br><span data-ttu-id="5527b-250">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-250">Min: `0`</span></span><br><span data-ttu-id="5527b-251">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="5527b-251">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="5527b-252">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-252">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-253">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="5527b-253">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="5527b-254">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-254">Optional string attribute.</span></span></p><p><span data-ttu-id="5527b-255">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="5527b-255">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="5527b-256">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="5527b-256">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="5527b-257">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="5527b-257">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="5527b-258">Al crearse el archivo de registro, el módulo crea las carpetas que se proporcionan en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-258">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="5527b-259">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo ( *.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="5527b-259">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="5527b-260">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="5527b-260">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="5527b-261">Atributo</span><span class="sxs-lookup"><span data-stu-id="5527b-261">Attribute</span></span> | <span data-ttu-id="5527b-262">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5527b-262">Description</span></span> | <span data-ttu-id="5527b-263">Default</span><span class="sxs-lookup"><span data-stu-id="5527b-263">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="5527b-264">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-264">Optional string attribute.</span></span></p><p><span data-ttu-id="5527b-265">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="5527b-265">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="5527b-266">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-266">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-267">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="5527b-267">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="5527b-268">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-268">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-269">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-269">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="5527b-270">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5527b-270">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="5527b-271">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-272">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-272">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="5527b-273">No se recomienda establecer `processesPerApplication`.</span><span class="sxs-lookup"><span data-stu-id="5527b-273">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="5527b-274">Este atributo se quitará en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="5527b-274">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="5527b-275">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="5527b-275">Default: `1`</span></span><br><span data-ttu-id="5527b-276">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="5527b-276">Min: `1`</span></span><br><span data-ttu-id="5527b-277">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="5527b-277">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="5527b-278">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="5527b-278">Required string attribute.</span></span></p><p><span data-ttu-id="5527b-279">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5527b-279">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="5527b-280">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="5527b-280">Relative paths are supported.</span></span> <span data-ttu-id="5527b-281">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="5527b-281">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="5527b-282">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-283">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="5527b-283">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="5527b-284">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="5527b-284">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="5527b-285">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="5527b-285">Default: `10`</span></span><br><span data-ttu-id="5527b-286">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-286">Min: `0`</span></span><br><span data-ttu-id="5527b-287">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="5527b-287">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="5527b-288">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-288">Optional timespan attribute.</span></span></p><p><span data-ttu-id="5527b-289">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="5527b-289">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="5527b-290">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="5527b-290">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="5527b-291">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-291">Default: `00:02:00`</span></span><br><span data-ttu-id="5527b-292">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-292">Min: `00:00:00`</span></span><br><span data-ttu-id="5527b-293">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="5527b-293">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="5527b-294">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-295">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="5527b-295">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="5527b-296">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="5527b-296">Default: `10`</span></span><br><span data-ttu-id="5527b-297">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-297">Min: `0`</span></span><br><span data-ttu-id="5527b-298">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="5527b-298">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="5527b-299">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-299">Optional integer attribute.</span></span></p><p><span data-ttu-id="5527b-300">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="5527b-300">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="5527b-301">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-301">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="5527b-302">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="5527b-302">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="5527b-303">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="5527b-303">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="5527b-304">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="5527b-304">Default: `120`</span></span><br><span data-ttu-id="5527b-305">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="5527b-305">Min: `0`</span></span><br><span data-ttu-id="5527b-306">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="5527b-306">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="5527b-307">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-307">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="5527b-308">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="5527b-308">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="5527b-309">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="5527b-309">Optional string attribute.</span></span></p><p><span data-ttu-id="5527b-310">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="5527b-310">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="5527b-311">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="5527b-311">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="5527b-312">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="5527b-312">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="5527b-313">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="5527b-313">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="5527b-314">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo ( *.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="5527b-314">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="5527b-315">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="5527b-315">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="5527b-316">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="5527b-316">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5527b-317">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="5527b-317">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="5527b-318">Especifique una variable de entorno con el elemento secundario `<environmentVariable>` de un elemento de la colección `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="5527b-318">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="5527b-319">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="5527b-319">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5527b-320">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="5527b-320">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="5527b-321">Especifique una variable de entorno con el elemento secundario `<environmentVariable>` de un elemento de la colección `<environmentVariables>`.</span><span class="sxs-lookup"><span data-stu-id="5527b-321">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="5527b-322">Las variables de entorno establecidas en esta sección entran en conflicto con las variables de entorno del sistema que tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="5527b-322">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="5527b-323">Si se establece una variable de entorno en el archivo *web.config* y en el nivel del sistema en Windows, el valor del archivo *web.config* se anexa al valor de la variable de entorno del sistema (por ejemplo, `ASPNETCORE_ENVIRONMENT: Development;Development`), que impide que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-323">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="5527b-324">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="5527b-324">The following example sets two environment variables.</span></span> <span data-ttu-id="5527b-325">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="5527b-325">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="5527b-326">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-326">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="5527b-327">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-327">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="5527b-328">Una alternativa a establecer directamente el entorno en *web.config* consiste en incluir la propiedad `<EnvironmentName>` en el perfil de publicación ( *.pubxml*) o el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5527b-328">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="5527b-329">Este método establece el entorno en *web.config* cuando se publica el proyecto:</span><span class="sxs-lookup"><span data-stu-id="5527b-329">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="5527b-330">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="5527b-330">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="5527b-331">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="5527b-331">app_offline.htm</span></span>

<span data-ttu-id="5527b-332">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="5527b-332">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="5527b-333">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="5527b-333">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="5527b-334">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="5527b-334">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="5527b-335">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-335">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-336">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="5527b-336">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="5527b-337">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-337">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="5527b-338">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="5527b-338">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-339">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-339">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="5527b-340">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="5527b-340">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="5527b-341">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="5527b-341">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="5527b-342">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="5527b-342">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="5527b-343">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="5527b-343">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="5527b-344">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="5527b-344">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5527b-345">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="5527b-345">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="5527b-346">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="5527b-346">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="5527b-347">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="5527b-347">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="5527b-349">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="5527b-349">Log creation and redirection</span></span>

<span data-ttu-id="5527b-350">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="5527b-350">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="5527b-351">Al crearse el archivo de registro, el módulo crea las carpetas de la ruta de acceso de `stdoutLogFile`.</span><span class="sxs-lookup"><span data-stu-id="5527b-351">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="5527b-352">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="5527b-352">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="5527b-353">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="5527b-353">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="5527b-354">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="5527b-354">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="5527b-355">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-355">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="5527b-356">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="5527b-356">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="5527b-357">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="5527b-357">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="5527b-358">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="5527b-358">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="5527b-359">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="5527b-359">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="5527b-360">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo ( *.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="5527b-360">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="5527b-361">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="5527b-361">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-362">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="5527b-362">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="5527b-363">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="5527b-363">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="5527b-364">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5527b-364">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="5527b-365">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="5527b-365">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="5527b-366">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="5527b-366">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="5527b-367">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="5527b-367">Enhanced diagnostic logs</span></span>

<span data-ttu-id="5527b-368">El módulo ASP.NET Core es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="5527b-368">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="5527b-369">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="5527b-369">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5527b-370">Al crearse el archivo de registro, el módulo crea las carpetas de la ruta de acceso (*logs* en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="5527b-370">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="5527b-371">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="5527b-371">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5527b-372">El módulo no crea automáticamente las carpetas de la ruta de acceso proporcionada al valor `<handlerSetting>` (*logs* en el ejemplo anterior), que deben existir previamente en la implementación.</span><span class="sxs-lookup"><span data-stu-id="5527b-372">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="5527b-373">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="5527b-373">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-374">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-374">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="5527b-375">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="5527b-375">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="5527b-376">ERROR</span><span class="sxs-lookup"><span data-stu-id="5527b-376">ERROR</span></span>
* <span data-ttu-id="5527b-377">WARNING</span><span class="sxs-lookup"><span data-stu-id="5527b-377">WARNING</span></span>
* <span data-ttu-id="5527b-378">INFO</span><span class="sxs-lookup"><span data-stu-id="5527b-378">INFO</span></span>
* <span data-ttu-id="5527b-379">TRACE</span><span class="sxs-lookup"><span data-stu-id="5527b-379">TRACE</span></span>

<span data-ttu-id="5527b-380">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="5527b-380">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="5527b-381">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="5527b-381">CONSOLE</span></span>
* <span data-ttu-id="5527b-382">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="5527b-382">EVENTLOG</span></span>
* <span data-ttu-id="5527b-383">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="5527b-383">FILE</span></span>

<span data-ttu-id="5527b-384">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="5527b-384">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="5527b-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="5527b-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="5527b-386">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="5527b-386">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="5527b-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="5527b-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="5527b-388">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="5527b-388">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="5527b-389">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="5527b-389">The size of the log isn't limited.</span></span> <span data-ttu-id="5527b-390">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5527b-390">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="5527b-391">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="5527b-391">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a><span data-ttu-id="5527b-392">Modificación del tamaño de la pila</span><span class="sxs-lookup"><span data-stu-id="5527b-392">Modify the stack size</span></span>

<span data-ttu-id="5527b-393">Configure el tamaño de la pila administrada mediante el valor `stackSize` en bytes.</span><span class="sxs-lookup"><span data-stu-id="5527b-393">Configure the managed stack size using the `stackSize` setting in bytes.</span></span> <span data-ttu-id="5527b-394">El tamaño predeterminado es `1048576` bytes (1 MB).</span><span class="sxs-lookup"><span data-stu-id="5527b-394">The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="5527b-395">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="5527b-395">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-396">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="5527b-396">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="5527b-397">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="5527b-397">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="5527b-398">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="5527b-398">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="5527b-399">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="5527b-399">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="5527b-400">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="5527b-400">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="5527b-401">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="5527b-401">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="5527b-402">El token de emparejamiento también se establece en un encabezado (`MS-ASPNETCORE-TOKEN`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="5527b-402">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="5527b-403">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="5527b-403">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="5527b-404">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="5527b-404">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="5527b-405">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="5527b-405">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="5527b-406">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-406">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="5527b-407">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="5527b-407">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="5527b-408">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **TrustedInstaller**.</span><span class="sxs-lookup"><span data-stu-id="5527b-408">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="5527b-409">Como la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador inicia un error de acceso denegado al intentar configurar los valores del módulo en el archivo *applicationHost.config* del recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="5527b-409">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5527b-410">Cuando se usa una configuración compartida de IIS en el mismo equipo que la instalación de IIS, ejecute el instalador del lote de hospedaje de ASP.NET Core con el parámetro `OPT_NO_SHARED_CONFIG_CHECK` establecido en `1`:</span><span class="sxs-lookup"><span data-stu-id="5527b-410">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="5527b-411">Cuando la ruta de acceso a la configuración compartida no se encuentra en el mismo equipo que la instalación de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5527b-411">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="5527b-412">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-412">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="5527b-413">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="5527b-413">Run the installer.</span></span>
1. <span data-ttu-id="5527b-414">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="5527b-414">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="5527b-415">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-415">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5527b-416">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5527b-416">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="5527b-417">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-417">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="5527b-418">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="5527b-418">Run the installer.</span></span>
1. <span data-ttu-id="5527b-419">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="5527b-419">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="5527b-420">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="5527b-420">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="5527b-421">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="5527b-421">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="5527b-422">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5527b-422">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="5527b-423">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="5527b-423">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="5527b-424">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="5527b-424">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="5527b-425">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="5527b-425">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="5527b-426">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="5527b-426">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="5527b-427">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="5527b-427">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="5527b-428">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="5527b-428">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="5527b-429">Module</span><span class="sxs-lookup"><span data-stu-id="5527b-429">Module</span></span>

<span data-ttu-id="5527b-430">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="5527b-430">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="5527b-431">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-431">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="5527b-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5527b-433">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-433">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="5527b-434">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-434">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="5527b-435">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="5527b-435">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="5527b-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="5527b-437">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-437">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5527b-438">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-438">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="5527b-439">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="5527b-439">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="5527b-440">Schema</span><span class="sxs-lookup"><span data-stu-id="5527b-440">Schema</span></span>

<span data-ttu-id="5527b-441">**IIS**</span><span class="sxs-lookup"><span data-stu-id="5527b-441">**IIS**</span></span>

* <span data-ttu-id="5527b-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="5527b-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5527b-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="5527b-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="5527b-444">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="5527b-444">**IIS Express**</span></span>

* <span data-ttu-id="5527b-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="5527b-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="5527b-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="5527b-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="5527b-447">Configuración</span><span class="sxs-lookup"><span data-stu-id="5527b-447">Configuration</span></span>

<span data-ttu-id="5527b-448">**IIS**</span><span class="sxs-lookup"><span data-stu-id="5527b-448">**IIS**</span></span>

* <span data-ttu-id="5527b-449">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="5527b-449">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="5527b-450">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="5527b-450">**IIS Express**</span></span>

* <span data-ttu-id="5527b-451">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="5527b-451">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="5527b-452">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="5527b-452">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="5527b-453">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="5527b-453">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5527b-454">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5527b-454">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="5527b-455">Repositorio GitHub del módulo ASP.NET Core (origen de referencia)</span><span class="sxs-lookup"><span data-stu-id="5527b-455">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
