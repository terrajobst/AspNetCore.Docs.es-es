---
title: Módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: f97d6f188bfcba6285cbd1fa91ce530e96395929
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249573"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="1ffea-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ffea-103">ASP.NET Core Module</span></span>

<span data-ttu-id="1ffea-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ffea-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1ffea-105">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="1ffea-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="1ffea-106">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="1ffea-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="1ffea-107">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="1ffea-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="1ffea-108">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="1ffea-108">Supported Windows versions:</span></span>

* <span data-ttu-id="1ffea-109">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="1ffea-109">Windows 7 or later</span></span>
* <span data-ttu-id="1ffea-110">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="1ffea-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1ffea-111">En el caso del hospedaje en proceso, el módulo usa el servidor HTTP de IIS (`IISHttpServer`), una implementación de servidor en proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="1ffea-112">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1ffea-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="1ffea-113">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="1ffea-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="1ffea-114">Modelos de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1ffea-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="1ffea-115">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="1ffea-115">In-process hosting model</span></span>

<span data-ttu-id="1ffea-116">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación con un valor de `InProcess` (el hospedaje fuera de proceso se establece con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="1ffea-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1ffea-117">No se admite el modelo de hospedaje en proceso para aplicaciones de ASP.NET Core que tienen como destino .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1ffea-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="1ffea-118">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="1ffea-119">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="1ffea-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="1ffea-120">En lugar del servidor HTTP de IIS (`IISHttpServer`) se usa el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1ffea-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="1ffea-121">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="1ffea-122">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="1ffea-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="1ffea-123">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-123">Use one app pool per app.</span></span>

* <span data-ttu-id="1ffea-124">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="1ffea-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1ffea-125">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="1ffea-126">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="1ffea-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="1ffea-127">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="1ffea-128">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="1ffea-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="1ffea-129">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="1ffea-129">Client disconnects are detected.</span></span> <span data-ttu-id="1ffea-130">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="1ffea-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="1ffea-131"><xref:System.IO.Directory.GetCurrentDirectory*> devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="1ffea-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="1ffea-132">Para conocer el código de ejemplo que establece el directorio actual de la aplicación, consulte la información sobre la [clase CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="1ffea-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="1ffea-133">Llame al método `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="1ffea-134">Las llamadas subsiguientes a <xref:System.IO.Directory.GetCurrentDirectory*> proporcionan el directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="1ffea-135">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="1ffea-135">Out-of-process hosting model</span></span>

<span data-ttu-id="1ffea-136">Para configurar una aplicación para el hospedaje fuera de proceso, use uno de los métodos siguientes en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="1ffea-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="1ffea-137">No especifique la propiedad `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="1ffea-138">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="1ffea-139">Establezca el valor de la propiedad `<AspNetCoreHostingModel>` en `OutOfProcess` (el hospedaje en proceso se establece con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="1ffea-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="1ffea-140">En lugar del servidor [Kestrel](xref:fundamentals/servers/kestrel) se usa el servidor HTTP de IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="1ffea-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="1ffea-141">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1ffea-141">Hosting model changes</span></span>

<span data-ttu-id="1ffea-142">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="1ffea-143">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="1ffea-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="1ffea-144">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1ffea-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="1ffea-145">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="1ffea-145">Process name</span></span>

<span data-ttu-id="1ffea-146">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="1ffea-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1ffea-147">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="1ffea-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="1ffea-148">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="1ffea-148">Supported Windows versions:</span></span>

* <span data-ttu-id="1ffea-149">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="1ffea-149">Windows 7 or later</span></span>
* <span data-ttu-id="1ffea-150">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="1ffea-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1ffea-151">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1ffea-151">The module only works with Kestrel.</span></span> <span data-ttu-id="1ffea-152">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="1ffea-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="1ffea-153">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="1ffea-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="1ffea-154">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="1ffea-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="1ffea-155">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="1ffea-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="1ffea-156">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="1ffea-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="1ffea-158">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="1ffea-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="1ffea-159">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1ffea-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="1ffea-160">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="1ffea-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="1ffea-161">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="1ffea-162">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="1ffea-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="1ffea-163">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="1ffea-164">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ffea-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="1ffea-165">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="1ffea-166">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1ffea-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="1ffea-167">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="1ffea-168">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="1ffea-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="1ffea-169">Para obtener más información sobre los módulos de IIS activos con el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="1ffea-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="1ffea-170">El módulo ASP.NET Core también puede:</span><span class="sxs-lookup"><span data-stu-id="1ffea-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="1ffea-171">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="1ffea-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="1ffea-172">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="1ffea-173">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="1ffea-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="1ffea-174">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ffea-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="1ffea-175">Para obtener instrucciones sobre cómo instalar y usar el módulo ASP.NET Core, consulte <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="1ffea-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="1ffea-176">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="1ffea-176">Configuration with web.config</span></span>

<span data-ttu-id="1ffea-177">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="1ffea-178">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="1ffea-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="1ffea-179">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="1ffea-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="1ffea-180">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="1ffea-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="1ffea-181">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="1ffea-182">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="1ffea-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="1ffea-183">Para obtener información sobre la configuración de aplicaciones secundarias de IIS, consulte <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="1ffea-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="1ffea-184">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="1ffea-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="1ffea-185">Atributo</span><span class="sxs-lookup"><span data-stu-id="1ffea-185">Attribute</span></span> | <span data-ttu-id="1ffea-186">Descripción</span><span class="sxs-lookup"><span data-stu-id="1ffea-186">Description</span></span> | <span data-ttu-id="1ffea-187">Default</span><span class="sxs-lookup"><span data-stu-id="1ffea-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1ffea-188">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-188">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-189">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1ffea-190">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-191">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1ffea-192">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-193">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1ffea-194">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="1ffea-195">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-195">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-196">Especifica el modelo de hospedaje como en proceso (`InProcess`) o fuera de proceso (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="1ffea-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="1ffea-197">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-198">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="1ffea-199">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="1ffea-200">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-200">Default: `1`</span></span><br><span data-ttu-id="1ffea-201">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-201">Min: `1`</span></span><br><span data-ttu-id="1ffea-202">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="1ffea-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="1ffea-203">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="1ffea-203">Required string attribute.</span></span></p><p><span data-ttu-id="1ffea-204">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="1ffea-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1ffea-205">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-205">Relative paths are supported.</span></span> <span data-ttu-id="1ffea-206">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1ffea-207">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-208">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1ffea-209">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="1ffea-210">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="1ffea-211">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-211">Default: `10`</span></span><br><span data-ttu-id="1ffea-212">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-212">Min: `0`</span></span><br><span data-ttu-id="1ffea-213">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="1ffea-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1ffea-214">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1ffea-215">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1ffea-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1ffea-216">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="1ffea-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="1ffea-217">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="1ffea-218">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="1ffea-219">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-219">Default: `00:02:00`</span></span><br><span data-ttu-id="1ffea-220">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-220">Min: `00:00:00`</span></span><br><span data-ttu-id="1ffea-221">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1ffea-222">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-223">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1ffea-224">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-224">Default: `10`</span></span><br><span data-ttu-id="1ffea-225">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-225">Min: `0`</span></span><br><span data-ttu-id="1ffea-226">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1ffea-227">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-228">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1ffea-229">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1ffea-230">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="1ffea-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1ffea-231">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="1ffea-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1ffea-232">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="1ffea-232">Default: `120`</span></span><br><span data-ttu-id="1ffea-233">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-233">Min: `0`</span></span><br><span data-ttu-id="1ffea-234">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1ffea-235">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-236">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1ffea-237">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-237">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-238">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1ffea-239">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1ffea-240">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1ffea-241">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="1ffea-241">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1ffea-242">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1ffea-243">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="1ffea-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="1ffea-244">Atributo</span><span class="sxs-lookup"><span data-stu-id="1ffea-244">Attribute</span></span> | <span data-ttu-id="1ffea-245">Descripción</span><span class="sxs-lookup"><span data-stu-id="1ffea-245">Description</span></span> | <span data-ttu-id="1ffea-246">Default</span><span class="sxs-lookup"><span data-stu-id="1ffea-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1ffea-247">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-247">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-248">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1ffea-249">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-250">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1ffea-251">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-252">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1ffea-253">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1ffea-254">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-255">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1ffea-256">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-256">Default: `1`</span></span><br><span data-ttu-id="1ffea-257">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-257">Min: `1`</span></span><br><span data-ttu-id="1ffea-258">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="1ffea-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1ffea-259">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="1ffea-259">Required string attribute.</span></span></p><p><span data-ttu-id="1ffea-260">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="1ffea-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1ffea-261">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-261">Relative paths are supported.</span></span> <span data-ttu-id="1ffea-262">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1ffea-263">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-264">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1ffea-265">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1ffea-266">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-266">Default: `10`</span></span><br><span data-ttu-id="1ffea-267">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-267">Min: `0`</span></span><br><span data-ttu-id="1ffea-268">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="1ffea-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1ffea-269">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1ffea-270">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1ffea-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1ffea-271">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="1ffea-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="1ffea-272">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-272">Default: `00:02:00`</span></span><br><span data-ttu-id="1ffea-273">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-273">Min: `00:00:00`</span></span><br><span data-ttu-id="1ffea-274">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1ffea-275">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-276">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1ffea-277">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-277">Default: `10`</span></span><br><span data-ttu-id="1ffea-278">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-278">Min: `0`</span></span><br><span data-ttu-id="1ffea-279">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1ffea-280">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-281">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1ffea-282">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1ffea-283">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="1ffea-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="1ffea-284">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="1ffea-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="1ffea-285">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="1ffea-285">Default: `120`</span></span><br><span data-ttu-id="1ffea-286">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-286">Min: `0`</span></span><br><span data-ttu-id="1ffea-287">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1ffea-288">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-289">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1ffea-290">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-290">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-291">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1ffea-292">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1ffea-293">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1ffea-294">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="1ffea-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1ffea-295">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1ffea-296">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="1ffea-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="1ffea-297">Atributo</span><span class="sxs-lookup"><span data-stu-id="1ffea-297">Attribute</span></span> | <span data-ttu-id="1ffea-298">Descripción</span><span class="sxs-lookup"><span data-stu-id="1ffea-298">Description</span></span> | <span data-ttu-id="1ffea-299">Default</span><span class="sxs-lookup"><span data-stu-id="1ffea-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="1ffea-300">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-300">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-301">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="1ffea-302">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-303">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="1ffea-304">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-305">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="1ffea-306">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1ffea-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="1ffea-307">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-308">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="1ffea-309">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-309">Default: `1`</span></span><br><span data-ttu-id="1ffea-310">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="1ffea-310">Min: `1`</span></span><br><span data-ttu-id="1ffea-311">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="1ffea-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="1ffea-312">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="1ffea-312">Required string attribute.</span></span></p><p><span data-ttu-id="1ffea-313">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="1ffea-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="1ffea-314">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-314">Relative paths are supported.</span></span> <span data-ttu-id="1ffea-315">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="1ffea-316">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-317">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="1ffea-318">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="1ffea-319">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-319">Default: `10`</span></span><br><span data-ttu-id="1ffea-320">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-320">Min: `0`</span></span><br><span data-ttu-id="1ffea-321">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="1ffea-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="1ffea-322">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="1ffea-323">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="1ffea-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="1ffea-324">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="1ffea-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="1ffea-325">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-325">Default: `00:02:00`</span></span><br><span data-ttu-id="1ffea-326">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-326">Min: `00:00:00`</span></span><br><span data-ttu-id="1ffea-327">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="1ffea-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="1ffea-328">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-329">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="1ffea-330">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="1ffea-330">Default: `10`</span></span><br><span data-ttu-id="1ffea-331">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-331">Min: `0`</span></span><br><span data-ttu-id="1ffea-332">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="1ffea-333">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="1ffea-334">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="1ffea-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="1ffea-335">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="1ffea-336">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="1ffea-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="1ffea-337">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="1ffea-337">Default: `120`</span></span><br><span data-ttu-id="1ffea-338">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="1ffea-338">Min: `0`</span></span><br><span data-ttu-id="1ffea-339">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="1ffea-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="1ffea-340">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="1ffea-341">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="1ffea-342">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="1ffea-342">Optional string attribute.</span></span></p><p><span data-ttu-id="1ffea-343">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="1ffea-344">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="1ffea-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="1ffea-345">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="1ffea-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="1ffea-346">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="1ffea-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1ffea-347">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="1ffea-348">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="1ffea-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="1ffea-349">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="1ffea-349">Setting environment variables</span></span>

<span data-ttu-id="1ffea-350">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="1ffea-351">Especifique una variable de entorno con el elemento secundario `environmentVariable` de un elemento de la colección `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-351">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="1ffea-352">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="1ffea-352">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="1ffea-353">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="1ffea-353">The following example sets two environment variables.</span></span> <span data-ttu-id="1ffea-354">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-354">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="1ffea-355">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-355">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="1ffea-356">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-356">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="1ffea-357">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="1ffea-357">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="1ffea-358">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1ffea-358">app_offline.htm</span></span>

<span data-ttu-id="1ffea-359">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="1ffea-359">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="1ffea-360">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="1ffea-360">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="1ffea-361">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-361">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="1ffea-362">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-362">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1ffea-363">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="1ffea-363">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="1ffea-364">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-364">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="1ffea-365">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="1ffea-365">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1ffea-366">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-366">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="1ffea-367">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-367">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="1ffea-368">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-368">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="1ffea-369">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-369">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="1ffea-370">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-370">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1ffea-371">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1ffea-371">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1ffea-372">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-372">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="1ffea-373">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-373">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="1ffea-374">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="1ffea-374">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="1ffea-376">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="1ffea-376">Log creation and redirection</span></span>

<span data-ttu-id="1ffea-377">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="1ffea-377">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="1ffea-378">Las carpetas de la ruta de acceso `stdoutLogFile` debe estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="1ffea-378">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="1ffea-379">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="1ffea-379">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="1ffea-380">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="1ffea-380">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="1ffea-381">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="1ffea-381">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="1ffea-382">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-382">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="1ffea-383">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="1ffea-383">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="1ffea-384">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="1ffea-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="1ffea-385">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="1ffea-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="1ffea-386">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="1ffea-386">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="1ffea-387">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="1ffea-387">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="1ffea-388">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-388">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1ffea-389">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="1ffea-389">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="1ffea-390">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="1ffea-390">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="1ffea-391">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1ffea-391">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="1ffea-392">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="1ffea-392">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="1ffea-393">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="1ffea-393">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="1ffea-394">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="1ffea-394">Enhanced diagnostic logs</span></span>

<span data-ttu-id="1ffea-395">El módulo ASP.NET Core que se proporciona es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="1ffea-395">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="1ffea-396">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="1ffea-396">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="1ffea-397">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-397">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="1ffea-398">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="1ffea-398">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="1ffea-399">ERROR</span><span class="sxs-lookup"><span data-stu-id="1ffea-399">ERROR</span></span>
* <span data-ttu-id="1ffea-400">WARNING</span><span class="sxs-lookup"><span data-stu-id="1ffea-400">WARNING</span></span>
* <span data-ttu-id="1ffea-401">INFO</span><span class="sxs-lookup"><span data-stu-id="1ffea-401">INFO</span></span>
* <span data-ttu-id="1ffea-402">TRACE</span><span class="sxs-lookup"><span data-stu-id="1ffea-402">TRACE</span></span>

<span data-ttu-id="1ffea-403">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="1ffea-403">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="1ffea-404">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="1ffea-404">CONSOLE</span></span>
* <span data-ttu-id="1ffea-405">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="1ffea-405">EVENTLOG</span></span>
* <span data-ttu-id="1ffea-406">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="1ffea-406">FILE</span></span>

<span data-ttu-id="1ffea-407">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="1ffea-407">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="1ffea-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="1ffea-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="1ffea-409">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="1ffea-409">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="1ffea-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="1ffea-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="1ffea-411">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="1ffea-411">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="1ffea-412">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="1ffea-412">The size of the log isn't limited.</span></span> <span data-ttu-id="1ffea-413">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="1ffea-413">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="1ffea-414">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-414">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="1ffea-415">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="1ffea-415">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1ffea-416">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="1ffea-416">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="1ffea-417">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="1ffea-417">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="1ffea-418">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="1ffea-418">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="1ffea-419">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="1ffea-419">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="1ffea-420">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="1ffea-420">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="1ffea-421">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="1ffea-421">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="1ffea-422">El token de emparejamiento también se establece en un encabezado (`MS-ASPNETCORE-TOKEN`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="1ffea-422">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="1ffea-423">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="1ffea-423">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="1ffea-424">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="1ffea-424">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="1ffea-425">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="1ffea-425">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="1ffea-426">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-426">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="1ffea-427">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="1ffea-427">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="1ffea-428">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="1ffea-428">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="1ffea-429">Dado que la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador recibe un error de acceso denegado al intentar configurar los valores del módulo en *applicationHost.config* en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="1ffea-429">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="1ffea-430">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="1ffea-430">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="1ffea-431">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-431">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="1ffea-432">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="1ffea-432">Run the installer.</span></span>
1. <span data-ttu-id="1ffea-433">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="1ffea-433">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="1ffea-434">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="1ffea-434">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="1ffea-435">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="1ffea-435">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="1ffea-436">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="1ffea-436">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="1ffea-437">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-437">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="1ffea-438">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-438">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="1ffea-439">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="1ffea-439">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="1ffea-440">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="1ffea-440">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="1ffea-441">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-441">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="1ffea-442">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="1ffea-442">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="1ffea-443">Module</span><span class="sxs-lookup"><span data-stu-id="1ffea-443">Module</span></span>

<span data-ttu-id="1ffea-444">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1ffea-444">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="1ffea-445">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-445">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="1ffea-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1ffea-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1ffea-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="1ffea-449">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="1ffea-449">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="1ffea-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="1ffea-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1ffea-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="1ffea-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="1ffea-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="1ffea-454">Schema</span><span class="sxs-lookup"><span data-stu-id="1ffea-454">Schema</span></span>

<span data-ttu-id="1ffea-455">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1ffea-455">**IIS**</span></span>

   * <span data-ttu-id="1ffea-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1ffea-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1ffea-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1ffea-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="1ffea-458">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1ffea-458">**IIS Express**</span></span>

   * <span data-ttu-id="1ffea-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="1ffea-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="1ffea-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="1ffea-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="1ffea-461">Configuración</span><span class="sxs-lookup"><span data-stu-id="1ffea-461">Configuration</span></span>

<span data-ttu-id="1ffea-462">**IIS**</span><span class="sxs-lookup"><span data-stu-id="1ffea-462">**IIS**</span></span>

   * <span data-ttu-id="1ffea-463">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1ffea-463">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="1ffea-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="1ffea-464">**IIS Express**</span></span>

   * <span data-ttu-id="1ffea-465">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="1ffea-465">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="1ffea-466">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="1ffea-466">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ffea-467">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1ffea-467">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="1ffea-468">Repositorio GitHub del módulo ASP.NET Core (origen de referencia)</span><span class="sxs-lookup"><span data-stu-id="1ffea-468">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>