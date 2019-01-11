---
title: Módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo configurar el módulo de ASP.NET Core para hospedar aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: dee4fe7a498d211cb8ef6a3c49017c3cc8a56847
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637864"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="2966d-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2966d-103">ASP.NET Core Module</span></span>

<span data-ttu-id="2966d-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2966d-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2966d-105">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="2966d-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="2966d-106">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2966d-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="2966d-107">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="2966d-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="2966d-108">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="2966d-108">Supported Windows versions:</span></span>

* <span data-ttu-id="2966d-109">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="2966d-109">Windows 7 or later</span></span>
* <span data-ttu-id="2966d-110">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="2966d-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2966d-111">En el caso del hospedaje en proceso, el módulo usa el servidor HTTP de IIS (`IISHttpServer`), una implementación de servidor en proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="2966d-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="2966d-112">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2966d-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="2966d-113">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2966d-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="2966d-114">Modelos de hospedaje</span><span class="sxs-lookup"><span data-stu-id="2966d-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="2966d-115">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="2966d-115">In-process hosting model</span></span>

<span data-ttu-id="2966d-116">Para configurar una aplicación para el hospedaje en proceso, agregue la propiedad `<AspNetCoreHostingModel>` al archivo de proyecto de la aplicación con un valor de `InProcess` (el hospedaje fuera de proceso se establece con `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="2966d-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2966d-117">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2966d-117">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="2966d-118">Al hospedar en proceso, se aplican las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="2966d-118">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="2966d-119">En lugar del servidor HTTP de IIS (`IISHttpServer`) se usa el servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2966d-119">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="2966d-120">El [atributo requestTimeout](#attributes-of-the-aspnetcore-element) no se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-120">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="2966d-121">No se admite el uso compartido de un grupo de aplicaciones entre aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2966d-121">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="2966d-122">Se usa un grupo de aplicaciones por aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-122">Use one app pool per app.</span></span>

* <span data-ttu-id="2966d-123">Cuando se usa [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) o se coloca manualmente un [archivo app_offline.htm en la implementación](xref:host-and-deploy/iis/index#locked-deployment-files), puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="2966d-123">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2966d-124">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-124">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="2966d-125">La arquitectura (valor de bits) de la aplicación y el runtime instalado (x64 o x86) deben coincidir con la arquitectura del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2966d-125">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="2966d-126">Si se configura el host de la aplicación manualmente con `WebHostBuilder` (no mediante [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) y la aplicación nunca se ejecuta directamente en el servidor de Kestrel (autohospedada), llame a `UseKestrel` antes de llamar a `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2966d-126">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="2966d-127">Si se invierte el orden, el host no se inicia.</span><span class="sxs-lookup"><span data-stu-id="2966d-127">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="2966d-128">Se detectan las desconexiones del cliente.</span><span class="sxs-lookup"><span data-stu-id="2966d-128">Client disconnects are detected.</span></span> <span data-ttu-id="2966d-129">El token de cancelación [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) se cancela cuando el cliente se desconecta.</span><span class="sxs-lookup"><span data-stu-id="2966d-129">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="2966d-130"><xref:System.IO.Directory.GetCurrentDirectory*> devuelve el directorio de trabajo del proceso iniciado por IIS en lugar del de la aplicación (por ejemplo, *C:\Windows\System32\inetsrv* para *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="2966d-130"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="2966d-131">Para conocer el código de ejemplo que establece el directorio actual de la aplicación, consulte la información sobre la [clase CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="2966d-131">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="2966d-132">Llame al método `SetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="2966d-132">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="2966d-133">Las llamadas subsiguientes a <xref:System.IO.Directory.GetCurrentDirectory*> proporcionan el directorio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-133">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="2966d-134">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="2966d-134">Out-of-process hosting model</span></span>

<span data-ttu-id="2966d-135">Para configurar una aplicación para el hospedaje fuera de proceso, use uno de los métodos siguientes en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="2966d-135">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="2966d-136">No especifique la propiedad `<AspNetCoreHostingModel>`.</span><span class="sxs-lookup"><span data-stu-id="2966d-136">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="2966d-137">Si la propiedad `<AspNetCoreHostingModel>` no está presente en el archivo, el valor predeterminado es `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="2966d-137">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="2966d-138">Establezca el valor de la propiedad `<AspNetCoreHostingModel>` en `OutOfProcess` (el hospedaje en proceso se establece con `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="2966d-138">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="2966d-139">En lugar del servidor [Kestrel](xref:fundamentals/servers/kestrel) se usa el servidor HTTP de IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="2966d-139">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="2966d-140">Cambios del modelo de hospedaje</span><span class="sxs-lookup"><span data-stu-id="2966d-140">Hosting model changes</span></span>

<span data-ttu-id="2966d-141">Si se modifica el valor `hostingModel` en el archivo *web.config* (se explica en la sección [Configuración con web.config](#configuration-with-webconfig)), el módulo recicla el proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="2966d-141">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="2966d-142">En IIS Express, el módulo no recicla el proceso de trabajo, sino que desencadena un cierre estable del proceso de IIS Express actual.</span><span class="sxs-lookup"><span data-stu-id="2966d-142">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="2966d-143">La siguiente solicitud a la aplicación genera un nuevo proceso de IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2966d-143">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="2966d-144">Nombre del proceso</span><span class="sxs-lookup"><span data-stu-id="2966d-144">Process name</span></span>

<span data-ttu-id="2966d-145">`Process.GetCurrentProcess().ProcessName` informa a `w3wp`/`iisexpress` (en proceso) o `dotnet` (fuera de proceso).</span><span class="sxs-lookup"><span data-stu-id="2966d-145">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2966d-146">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="2966d-146">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="2966d-147">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="2966d-147">Supported Windows versions:</span></span>

* <span data-ttu-id="2966d-148">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="2966d-148">Windows 7 or later</span></span>
* <span data-ttu-id="2966d-149">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="2966d-149">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="2966d-150">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2966d-150">The module only works with Kestrel.</span></span> <span data-ttu-id="2966d-151">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2966d-151">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="2966d-152">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="2966d-152">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="2966d-153">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="2966d-153">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="2966d-154">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="2966d-154">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="2966d-155">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="2966d-155">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="2966d-157">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="2966d-157">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="2966d-158">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="2966d-158">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="2966d-159">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="2966d-159">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="2966d-160">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="2966d-160">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="2966d-161">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="2966d-161">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="2966d-162">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2966d-162">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="2966d-163">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2966d-163">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="2966d-164">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-164">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="2966d-165">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2966d-165">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="2966d-166">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-166">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="2966d-167">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="2966d-167">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="2966d-168">Para obtener más información sobre los módulos de IIS activos con el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="2966d-168">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="2966d-169">El módulo ASP.NET Core también puede:</span><span class="sxs-lookup"><span data-stu-id="2966d-169">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="2966d-170">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="2966d-170">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="2966d-171">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="2966d-171">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="2966d-172">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="2966d-172">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="2966d-173">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2966d-173">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="2966d-174">Para obtener instrucciones sobre cómo instalar y usar el módulo ASP.NET Core, consulte <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="2966d-174">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="2966d-175">Configuración con web.config</span><span class="sxs-lookup"><span data-stu-id="2966d-175">Configuration with web.config</span></span>

<span data-ttu-id="2966d-176">El módulo ASP.NET Core se configura con la sección `aspNetCore` del nodo `system.webServer` del archivo *web.config* del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-176">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="2966d-177">El siguiente archivo *web.config* se publica para una [implementación dependiente del marco](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) y configura el módulo ASP.NET Core para controlar las solicitudes de sitios:</span><span class="sxs-lookup"><span data-stu-id="2966d-177">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="2966d-178">El siguiente archivo *web.config* se publica para una [implementación independiente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="2966d-178">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="2966d-179">La propiedad <xref:System.Configuration.SectionInformation.InheritInChildApplications*> está establecida en `false` para indicar que las aplicaciones que residen en un subdirectorio de la aplicación no heredan la configuración especificada en el elemento [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location).</span><span class="sxs-lookup"><span data-stu-id="2966d-179">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="2966d-180">Cuando se implementa una aplicación en [Azure App Service](https://azure.microsoft.com/services/app-service/), la ruta de acceso de `stdoutLogFile` se establece en `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="2966d-180">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="2966d-181">La ruta de acceso guarda los registros de stdout en la carpeta *LogFiles*, que es una ubicación que el servicio crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="2966d-181">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="2966d-182">Para obtener información sobre la configuración de aplicaciones secundarias de IIS, consulte <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="2966d-182">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="2966d-183">Atributos del elemento aspNetCore</span><span class="sxs-lookup"><span data-stu-id="2966d-183">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="2966d-184">Atributo</span><span class="sxs-lookup"><span data-stu-id="2966d-184">Attribute</span></span> | <span data-ttu-id="2966d-185">Descripción</span><span class="sxs-lookup"><span data-stu-id="2966d-185">Description</span></span> | <span data-ttu-id="2966d-186">Default</span><span class="sxs-lookup"><span data-stu-id="2966d-186">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2966d-187">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-187">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-188">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-188">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2966d-189">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-189">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-190">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="2966d-190">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2966d-191">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-192">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-192">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2966d-193">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-193">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="2966d-194">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-194">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-195">Especifica el modelo de hospedaje como en proceso (`InProcess`) o fuera de proceso (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="2966d-195">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="2966d-196">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-196">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-197">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-197">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="2966d-198">&dagger;En el hospedaje en proceso, el valor está limitado a `1`.</span><span class="sxs-lookup"><span data-stu-id="2966d-198">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="2966d-199">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-199">Default: `1`</span></span><br><span data-ttu-id="2966d-200">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-200">Min: `1`</span></span><br><span data-ttu-id="2966d-201">Máximo: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="2966d-201">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="2966d-202">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="2966d-202">Required string attribute.</span></span></p><p><span data-ttu-id="2966d-203">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2966d-203">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2966d-204">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="2966d-204">Relative paths are supported.</span></span> <span data-ttu-id="2966d-205">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-205">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2966d-206">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-206">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-207">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-207">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2966d-208">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-208">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="2966d-209">No admitido con el hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-209">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="2966d-210">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-210">Default: `10`</span></span><br><span data-ttu-id="2966d-211">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-211">Min: `0`</span></span><br><span data-ttu-id="2966d-212">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="2966d-212">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2966d-213">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-213">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2966d-214">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2966d-214">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2966d-215">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="2966d-215">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="2966d-216">No se aplica al hospedaje en proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-216">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="2966d-217">En el hospedaje en proceso, el módulo espera a que la aplicación procese la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-217">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="2966d-218">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-218">Default: `00:02:00`</span></span><br><span data-ttu-id="2966d-219">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-219">Min: `00:00:00`</span></span><br><span data-ttu-id="2966d-220">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-220">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2966d-221">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-221">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-222">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2966d-222">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2966d-223">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-223">Default: `10`</span></span><br><span data-ttu-id="2966d-224">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-224">Min: `0`</span></span><br><span data-ttu-id="2966d-225">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="2966d-225">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2966d-226">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-226">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-227">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="2966d-227">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2966d-228">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-228">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2966d-229">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="2966d-229">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2966d-230">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="2966d-230">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2966d-231">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="2966d-231">Default: `120`</span></span><br><span data-ttu-id="2966d-232">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-232">Min: `0`</span></span><br><span data-ttu-id="2966d-233">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="2966d-233">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2966d-234">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-234">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-235">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-235">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2966d-236">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-236">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-237">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-237">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2966d-238">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-238">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2966d-239">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="2966d-239">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2966d-240">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="2966d-240">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2966d-241">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-241">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2966d-242">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="2966d-242">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="2966d-243">Atributo</span><span class="sxs-lookup"><span data-stu-id="2966d-243">Attribute</span></span> | <span data-ttu-id="2966d-244">Descripción</span><span class="sxs-lookup"><span data-stu-id="2966d-244">Description</span></span> | <span data-ttu-id="2966d-245">Default</span><span class="sxs-lookup"><span data-stu-id="2966d-245">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2966d-246">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-246">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-247">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-247">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2966d-248">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-248">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-249">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="2966d-249">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2966d-250">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-250">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-251">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-251">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2966d-252">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-252">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2966d-253">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-253">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-254">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-254">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2966d-255">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-255">Default: `1`</span></span><br><span data-ttu-id="2966d-256">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-256">Min: `1`</span></span><br><span data-ttu-id="2966d-257">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="2966d-257">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2966d-258">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="2966d-258">Required string attribute.</span></span></p><p><span data-ttu-id="2966d-259">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2966d-259">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2966d-260">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="2966d-260">Relative paths are supported.</span></span> <span data-ttu-id="2966d-261">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-261">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2966d-262">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-262">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-263">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-263">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2966d-264">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-264">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2966d-265">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-265">Default: `10`</span></span><br><span data-ttu-id="2966d-266">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-266">Min: `0`</span></span><br><span data-ttu-id="2966d-267">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="2966d-267">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2966d-268">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-268">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2966d-269">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2966d-269">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2966d-270">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.1 o posterior, el valor `requestTimeout` se especifica en horas, minutos y segundos.</span><span class="sxs-lookup"><span data-stu-id="2966d-270">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="2966d-271">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-271">Default: `00:02:00`</span></span><br><span data-ttu-id="2966d-272">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-272">Min: `00:00:00`</span></span><br><span data-ttu-id="2966d-273">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-273">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2966d-274">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-274">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-275">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2966d-275">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2966d-276">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-276">Default: `10`</span></span><br><span data-ttu-id="2966d-277">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-277">Min: `0`</span></span><br><span data-ttu-id="2966d-278">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="2966d-278">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2966d-279">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-279">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-280">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="2966d-280">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2966d-281">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-281">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2966d-282">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="2966d-282">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="2966d-283">Un valor de 0 (cero) **no** se considera un tiempo de expiración infinito.</span><span class="sxs-lookup"><span data-stu-id="2966d-283">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="2966d-284">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="2966d-284">Default: `120`</span></span><br><span data-ttu-id="2966d-285">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-285">Min: `0`</span></span><br><span data-ttu-id="2966d-286">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="2966d-286">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2966d-287">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-287">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-288">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-288">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2966d-289">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-289">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-290">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-290">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2966d-291">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-291">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2966d-292">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="2966d-292">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2966d-293">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="2966d-293">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2966d-294">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-294">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2966d-295">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="2966d-295">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="2966d-296">Atributo</span><span class="sxs-lookup"><span data-stu-id="2966d-296">Attribute</span></span> | <span data-ttu-id="2966d-297">Descripción</span><span class="sxs-lookup"><span data-stu-id="2966d-297">Description</span></span> | <span data-ttu-id="2966d-298">Default</span><span class="sxs-lookup"><span data-stu-id="2966d-298">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="2966d-299">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-299">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-300">Argumentos para el archivo ejecutable especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-300">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="2966d-301">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-301">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-302">Si es true, la página **502.5 - Error en el proceso** se suprime, y tiene prioridad la página de código de estado 502 configurada en *web.config*.</span><span class="sxs-lookup"><span data-stu-id="2966d-302">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="2966d-303">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-303">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-304">Si es true, el token se reenvía al proceso secundario que escucha en % ASPNETCORE_PORT % como un encabezado "MS-ASPNETCORE-WINAUTHTOKEN" por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-304">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="2966d-305">Es responsabilidad de dicho proceso llamar a CloseHandle en este token por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2966d-305">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="2966d-306">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-306">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-307">Especifica el número de instancias del proceso especificado en el valor **processPath** que pueden rotarse por aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-307">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="2966d-308">Valor predeterminado: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-308">Default: `1`</span></span><br><span data-ttu-id="2966d-309">Mínimo: `1`</span><span class="sxs-lookup"><span data-stu-id="2966d-309">Min: `1`</span></span><br><span data-ttu-id="2966d-310">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="2966d-310">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="2966d-311">Atributo de cadena necesario.</span><span class="sxs-lookup"><span data-stu-id="2966d-311">Required string attribute.</span></span></p><p><span data-ttu-id="2966d-312">Ruta de acceso al archivo ejecutable que inicia un proceso que escucha las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2966d-312">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="2966d-313">No se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="2966d-313">Relative paths are supported.</span></span> <span data-ttu-id="2966d-314">Si la ruta de acceso comienza con `.`, se considera que es relativa a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-314">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="2966d-315">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-315">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-316">Especifica el número de veces que el proceso indicado en **processPath** puede bloquearse por minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-316">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="2966d-317">Si se supera este límite, el módulo deja de iniciar el proceso durante lo que resta del minuto.</span><span class="sxs-lookup"><span data-stu-id="2966d-317">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="2966d-318">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-318">Default: `10`</span></span><br><span data-ttu-id="2966d-319">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-319">Min: `0`</span></span><br><span data-ttu-id="2966d-320">Máximo: `100`</span><span class="sxs-lookup"><span data-stu-id="2966d-320">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="2966d-321">Atributo timespan opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-321">Optional timespan attribute.</span></span></p><p><span data-ttu-id="2966d-322">Especifica el tiempo que el módulo ASP.NET Core espera una respuesta del proceso que escucha en % ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="2966d-322">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="2966d-323">En las versiones del módulo ASP.NET Core que se envían con la versión de ASP.NET Core 2.0 o anterior, `requestTimeout` solo se debe especificar en minutos enteros, si no, adopta el valor predeterminado de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="2966d-323">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="2966d-324">Valor predeterminado: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-324">Default: `00:02:00`</span></span><br><span data-ttu-id="2966d-325">Mínimo: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-325">Min: `00:00:00`</span></span><br><span data-ttu-id="2966d-326">Máximo: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="2966d-326">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="2966d-327">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-327">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-328">Tiempo en segundos que el módulo espera a que se cierre correctamente el archivo ejecutable cuando se detecta el archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2966d-328">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="2966d-329">Valor predeterminado: `10`</span><span class="sxs-lookup"><span data-stu-id="2966d-329">Default: `10`</span></span><br><span data-ttu-id="2966d-330">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-330">Min: `0`</span></span><br><span data-ttu-id="2966d-331">Máximo: `600`</span><span class="sxs-lookup"><span data-stu-id="2966d-331">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="2966d-332">Atributo integer opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-332">Optional integer attribute.</span></span></p><p><span data-ttu-id="2966d-333">Tiempo en segundos que espera el módulo a que el archivo ejecutable inicie u proceso que escucha en el puerto.</span><span class="sxs-lookup"><span data-stu-id="2966d-333">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="2966d-334">Si se supera este límite de tiempo, el módulo termina el proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-334">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="2966d-335">El módulo intenta reiniciar el proceso cuando se recibe una nueva solicitud y lo sigue intentando en las sucesivas solicitudes entrantes a no ser que la aplicación no pueda iniciar **rapidFailsPerMinute** un número de veces en el último minuto acumulado.</span><span class="sxs-lookup"><span data-stu-id="2966d-335">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="2966d-336">Valor predeterminado: `120`</span><span class="sxs-lookup"><span data-stu-id="2966d-336">Default: `120`</span></span><br><span data-ttu-id="2966d-337">Mínimo: `0`</span><span class="sxs-lookup"><span data-stu-id="2966d-337">Min: `0`</span></span><br><span data-ttu-id="2966d-338">Máximo: `3600`</span><span class="sxs-lookup"><span data-stu-id="2966d-338">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="2966d-339">Atributo Boolean opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-339">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="2966d-340">Si es true, **stdout** y **stderr** en el proceso especificado en **processPath** se redirigen al archivo especificado en **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-340">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="2966d-341">Atributo de cadena opcional.</span><span class="sxs-lookup"><span data-stu-id="2966d-341">Optional string attribute.</span></span></p><p><span data-ttu-id="2966d-342">Especifica la ruta de acceso relativa o absoluta para la que se registran **stdout** y **stderr** desde el proceso especificado en **processPath**.</span><span class="sxs-lookup"><span data-stu-id="2966d-342">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="2966d-343">Las rutas de acceso relativas son relativas a la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="2966d-343">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="2966d-344">Cualquier ruta de acceso que se inicia con `.` es relativa a la raíz del sitio y todas las demás rutas de acceso se tratan como absolutas.</span><span class="sxs-lookup"><span data-stu-id="2966d-344">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="2966d-345">Las carpetas que se proporcionan en la ruta de acceso deben estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="2966d-345">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2966d-346">Mediante delimitadores se agrega una marca de tiempo, un identificador de proceso y una extensión de archivo (*.log*) al último segmento de la ruta de acceso **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="2966d-346">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="2966d-347">Si `.\logs\stdout` se proporciona como valor, se guarda un registro de ejemplo de stdout como *stdout_20180205194132_1934.log* en la carpeta *logs*, cuando se guarda el 5/2/2018 a las 19:41:32 con un identificador de proceso de 1934.</span><span class="sxs-lookup"><span data-stu-id="2966d-347">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="2966d-348">Configuración de las variables de entorno</span><span class="sxs-lookup"><span data-stu-id="2966d-348">Setting environment variables</span></span>

<span data-ttu-id="2966d-349">Se pueden especificar variables de entorno para el proceso en el atributo `processPath`.</span><span class="sxs-lookup"><span data-stu-id="2966d-349">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="2966d-350">Especifique una variable de entorno con el elemento secundario `environmentVariable` de un elemento de la colección `environmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="2966d-350">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="2966d-351">Las variables de entorno establecidas en esta sección tienen prioridad sobre las variables del entorno del sistema.</span><span class="sxs-lookup"><span data-stu-id="2966d-351">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="2966d-352">En el ejemplo siguiente se establecen dos variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2966d-352">The following example sets two environment variables.</span></span> <span data-ttu-id="2966d-353">`ASPNETCORE_ENVIRONMENT` configura el entorno de la aplicación como `Development`.</span><span class="sxs-lookup"><span data-stu-id="2966d-353">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="2966d-354">Un desarrollador puede establecer temporalmente este valor en el archivo *web.config* con el fin de forzar a que se cargue la [página de excepciones del desarrollador](xref:fundamentals/error-handling) al depurar una excepción de aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-354">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="2966d-355">`CONFIG_DIR` es un ejemplo de una variable de entorno definida por el usuario, donde el desarrollador ha escrito código que lee el valor al inicio para formar una ruta de acceso destinada a la carga del archivo de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-355">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="2966d-356">Establezca solo la variable de entorno `ASPNETCORE_ENVIRONMENT` en `Development` en servidores de ensayo y pruebas a los que no puedan acceder redes que no son de confianza, como Internet.</span><span class="sxs-lookup"><span data-stu-id="2966d-356">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="2966d-357">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="2966d-357">app_offline.htm</span></span>

<span data-ttu-id="2966d-358">Si un archivo con el nombre *app_offline.htm* se detecta en el directorio raíz de una aplicación, el módulo ASP.NET Core intenta cerrar correctamente la aplicación y deja de procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="2966d-358">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="2966d-359">Si la aplicación se sigue ejecutando después del número definido en `shutdownTimeLimit`, el módulo ASP.NET Core termina el proceso en ejecución.</span><span class="sxs-lookup"><span data-stu-id="2966d-359">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="2966d-360">Mientras el archivo *app_offline.htm* existe, el módulo ASP.NET Core responde a solicitudes con la devolución del contenido del archivo *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="2966d-360">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="2966d-361">Cuando se quita el archivo *app_offline.htm*, la solicitud siguiente inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-361">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2966d-362">Al usar el modelo de hospedaje fuera de proceso, puede que la aplicación no se apague inmediatamente si hay una conexión abierta.</span><span class="sxs-lookup"><span data-stu-id="2966d-362">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="2966d-363">Por ejemplo, una conexión WebSocket puede retrasar el apagado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-363">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="2966d-364">Página de errores de inicio</span><span class="sxs-lookup"><span data-stu-id="2966d-364">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2966d-365">Tanto el hospedaje en proceso como el hospedaje fuera de proceso generan páginas de error personalizado cuando se produce un error al iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-365">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="2966d-366">Si el módulo ASP.NET Core no logra encontrar el controlador de solicitudes en proceso o fuera de proceso, aparecerá la página de código de estado *500.0 - Error de carga de controlador en proceso/fuera de proceso*.</span><span class="sxs-lookup"><span data-stu-id="2966d-366">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="2966d-367">Para el hospedaje en proceso, si el módulo ASP.NET Core no logra iniciar la aplicación, aparecerá la página de código de estado *500.30 - Error de inicio*.</span><span class="sxs-lookup"><span data-stu-id="2966d-367">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="2966d-368">En cuanto al hospedaje fuera de proceso, si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparecerá la página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="2966d-368">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="2966d-369">Para suprimir esta página y volver a la página de código de estado 5xx de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2966d-369">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2966d-370">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2966d-370">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2966d-371">Si el módulo ASP.NET Core no es capaz de iniciar el proceso de back-end o este se inicia pero no puede escuchar en el puerto configurado, aparece una página de código de estado *502.5 - Error de proceso*.</span><span class="sxs-lookup"><span data-stu-id="2966d-371">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="2966d-372">Para suprimir esta página y volver a la página de código de estado 502 de IIS predeterminada, use el atributo `disableStartUpErrorPage`.</span><span class="sxs-lookup"><span data-stu-id="2966d-372">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="2966d-373">Para obtener más información sobre cómo configurar los mensajes de error personalizados, consulte [Errores HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="2966d-373">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![Página de códigos de estado 502.5 Error de proceso](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="2966d-375">Creación y redireccionamiento de registros</span><span class="sxs-lookup"><span data-stu-id="2966d-375">Log creation and redirection</span></span>

<span data-ttu-id="2966d-376">El módulo ASP.NET Core redirige los resultados de consola stdout y stderr al disco si se establecen los atributos `stdoutLogEnabled` y `stdoutLogFile` del elemento `aspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="2966d-376">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="2966d-377">Las carpetas de la ruta de acceso `stdoutLogFile` debe estar en orden para que el módulo cree el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="2966d-377">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="2966d-378">El grupo de aplicaciones debe tener acceso de escritura a la ubicación en la que se escriben los registros (use `IIS AppPool\<app_pool_name>` para proporcionar permiso de escritura).</span><span class="sxs-lookup"><span data-stu-id="2966d-378">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="2966d-379">Los registros no se rotan, a no ser que se produzca un reinicio o reciclaje del proceso.</span><span class="sxs-lookup"><span data-stu-id="2966d-379">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="2966d-380">Es responsabilidad del proveedor de servicios de hospedaje limitar el espacio en disco que consumen los registros.</span><span class="sxs-lookup"><span data-stu-id="2966d-380">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="2966d-381">El uso del registro de stdout solo se recomienda para solucionar problemas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-381">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="2966d-382">No use el registro de stdout con fines de registro de aplicaciones general.</span><span class="sxs-lookup"><span data-stu-id="2966d-382">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="2966d-383">Para el registro rutinario en una aplicación ASP.NET Core, use una biblioteca de registro que limite el tamaño del archivo de registro y realice la rotación de los registros.</span><span class="sxs-lookup"><span data-stu-id="2966d-383">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="2966d-384">Para más información, consulte los [proveedores de registro de terceros](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="2966d-384">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="2966d-385">Cuando se crea el archivo de registro, se agregan automáticamente una marca de tiempo y una extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="2966d-385">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="2966d-386">El nombre del archivo de registro se forma mediante la anexión de la marca de tiempo, el identificador de proceso y la extensión de archivo (*.log*) al último segmento de la ruta de acceso `stdoutLogFile` (normalmente *stdout*) delimitados por caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="2966d-386">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="2966d-387">Si la ruta de acceso de `stdoutLogFile` finaliza con *stdout*, el registro de una aplicación con un PID de 1934 creado el 5/2/2018 a las 19:42:32 tiene el nombre de archivo *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="2966d-387">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2966d-388">Si `stdoutLogEnabled` es falso, los errores que se produzcan al iniciar la aplicación se registrarán y se emitirán en el registro de eventos hasta un máximo de 30 KB.</span><span class="sxs-lookup"><span data-stu-id="2966d-388">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="2966d-389">Después del inicio, se descartarán los registros adicionales.</span><span class="sxs-lookup"><span data-stu-id="2966d-389">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="2966d-390">El elemento de ejemplo siguiente, `aspNetCore`, configura el registro de stdout para una aplicación hospedada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2966d-390">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="2966d-391">Una ruta de acceso local o una ruta de acceso de recurso compartido de red son aceptables para el registro local.</span><span class="sxs-lookup"><span data-stu-id="2966d-391">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="2966d-392">Confirme que la identidad del usuario de AppPool tenga permiso para escribir en la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="2966d-392">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="2966d-393">Registros de diagnóstico mejorados</span><span class="sxs-lookup"><span data-stu-id="2966d-393">Enhanced diagnostic logs</span></span>

<span data-ttu-id="2966d-394">El módulo ASP.NET Core que se proporciona es configurable para proporcionar registros de diagnóstico mejorados.</span><span class="sxs-lookup"><span data-stu-id="2966d-394">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="2966d-395">Agregue el elemento `<handlerSettings>` al elemento `<aspNetCore>` de *web.config*. Al establecer `debugLevel` en `TRACE` se expone una fidelidad mayor de información de diagnóstico:</span><span class="sxs-lookup"><span data-stu-id="2966d-395">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="2966d-396">Los valores de nivel de depuración (`debugLevel`) pueden incluir el nivel y la ubicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-396">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="2966d-397">Niveles (en orden de menos a más detallado):</span><span class="sxs-lookup"><span data-stu-id="2966d-397">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="2966d-398">ERROR</span><span class="sxs-lookup"><span data-stu-id="2966d-398">ERROR</span></span>
* <span data-ttu-id="2966d-399">WARNING</span><span class="sxs-lookup"><span data-stu-id="2966d-399">WARNING</span></span>
* <span data-ttu-id="2966d-400">INFO</span><span class="sxs-lookup"><span data-stu-id="2966d-400">INFO</span></span>
* <span data-ttu-id="2966d-401">TRACE</span><span class="sxs-lookup"><span data-stu-id="2966d-401">TRACE</span></span>

<span data-ttu-id="2966d-402">Ubicaciones (se permiten varias ubicaciones):</span><span class="sxs-lookup"><span data-stu-id="2966d-402">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="2966d-403">CONSOLE</span><span class="sxs-lookup"><span data-stu-id="2966d-403">CONSOLE</span></span>
* <span data-ttu-id="2966d-404">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="2966d-404">EVENTLOG</span></span>
* <span data-ttu-id="2966d-405">ARCHIVO</span><span class="sxs-lookup"><span data-stu-id="2966d-405">FILE</span></span>

<span data-ttu-id="2966d-406">También se puede proporcionar la configuración de controlador a través de variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="2966d-406">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="2966d-407">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Ruta de acceso al archivo de registro de depuración.</span><span class="sxs-lookup"><span data-stu-id="2966d-407">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="2966d-408">(El valor predeterminado es *aspnetcore-debug.log*)</span><span class="sxs-lookup"><span data-stu-id="2966d-408">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="2966d-409">`ASPNETCORE_MODULE_DEBUG` &ndash; Valor de nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="2966d-409">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="2966d-410">**No** deje habilitado el registro de depuración más tiempo del necesario en la implementación para solucionar un problema.</span><span class="sxs-lookup"><span data-stu-id="2966d-410">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="2966d-411">El tamaño del registro no es limitado.</span><span class="sxs-lookup"><span data-stu-id="2966d-411">The size of the log isn't limited.</span></span> <span data-ttu-id="2966d-412">Dejar habilitado el registro de depuración puede agotar el espacio disponible en disco y bloquear el servidor o el servicio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="2966d-412">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="2966d-413">Consulte [Configuración con web.config](#configuration-with-webconfig) para ver un ejemplo del elemento `aspNetCore` en el archivo *web.config*.</span><span class="sxs-lookup"><span data-stu-id="2966d-413">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="2966d-414">La configuración de proxy usa el protocolo HTTP y un token de emparejamiento</span><span class="sxs-lookup"><span data-stu-id="2966d-414">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="2966d-415">*Solo se aplica al hospedaje fuera de proceso.*</span><span class="sxs-lookup"><span data-stu-id="2966d-415">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="2966d-416">El proxy creado entre el módulo ASP.NET Core y Kestrel usa el protocolo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2966d-416">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="2966d-417">El uso de HTTP optimiza el rendimiento, ya que el tráfico entre el módulo y Kestrel se realiza en una dirección de bucle invertido fuera de la interfaz de red.</span><span class="sxs-lookup"><span data-stu-id="2966d-417">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="2966d-418">No hay ningún riesgo de interceptación del tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="2966d-418">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="2966d-419">Un token de emparejamiento sirve para garantizar que las solicitudes recibidas por Kestrel se redirigieron mediante proxy por IIS y no procedieron de otra fuente.</span><span class="sxs-lookup"><span data-stu-id="2966d-419">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="2966d-420">El módulo crea el token de emparejamiento y lo establece en una variable de entorno (`ASPNETCORE_TOKEN`).</span><span class="sxs-lookup"><span data-stu-id="2966d-420">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="2966d-421">El token de emparejamiento también se establece en un encabezado (`MS-ASPNETCORE-TOKEN`) en cada solicitud redirigida mediante proxy.</span><span class="sxs-lookup"><span data-stu-id="2966d-421">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="2966d-422">El middleware de IIS comprueba cada solicitud recibida para confirmar que el valor del encabezado del token de emparejamiento coincida con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2966d-422">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="2966d-423">Si los valores del token no coinciden, la solicitud se registra y se rechaza.</span><span class="sxs-lookup"><span data-stu-id="2966d-423">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="2966d-424">No se puede acceder a la variable de entorno del token de emparejamiento y al tráfico entre el módulo y Kestrel desde una ubicación fuera del servidor.</span><span class="sxs-lookup"><span data-stu-id="2966d-424">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="2966d-425">Sin conocer el valor del token de emparejamiento, un atacante no puede enviar solicitudes que omitan la comprobación en el middleware de IIS.</span><span class="sxs-lookup"><span data-stu-id="2966d-425">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="2966d-426">El módulo ASP.NET Core con una configuración compartida de IIS</span><span class="sxs-lookup"><span data-stu-id="2966d-426">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="2966d-427">El instalador del módulo ASP.NET Core se ejecuta con los privilegios de la cuenta **SYSTEM**.</span><span class="sxs-lookup"><span data-stu-id="2966d-427">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="2966d-428">Dado que la cuenta local del sistema no tiene permiso de modificación en la ruta de acceso de recurso compartido que se usa en la configuración compartida de IIS, el instalador recibe un error de acceso denegado al intentar configurar los valores del módulo en *applicationHost.config* en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="2966d-428">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="2966d-429">Al usar una configuración compartida de IIS, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="2966d-429">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="2966d-430">Deshabilite la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="2966d-430">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="2966d-431">Ejecute el instalador.</span><span class="sxs-lookup"><span data-stu-id="2966d-431">Run the installer.</span></span>
1. <span data-ttu-id="2966d-432">Exporte el archivo *applicationHost.config* actualizado al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="2966d-432">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="2966d-433">Vuelva a habilitar la configuración compartida de IIS.</span><span class="sxs-lookup"><span data-stu-id="2966d-433">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="2966d-434">Versión del módulo y registros del instalador de la agrupación de hospedaje</span><span class="sxs-lookup"><span data-stu-id="2966d-434">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="2966d-435">Para determinar la versión instalada del módulo ASP.NET Core, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="2966d-435">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="2966d-436">En el sistema de hospedaje, vaya a *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="2966d-436">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="2966d-437">Busque el archivo *aspnetcore.dll*.</span><span class="sxs-lookup"><span data-stu-id="2966d-437">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="2966d-438">Haga clic con el botón derecho en el archivo y seleccione **Propiedades** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="2966d-438">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="2966d-439">Seleccione la pestaña **Detalles**. La **versión del archivo** y la **versión del producto** representan la versión instalada del módulo.</span><span class="sxs-lookup"><span data-stu-id="2966d-439">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="2966d-440">Los registros del instalador de la agrupación de hospedaje del módulo se encuentran en *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. El archivo se llama *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="2966d-440">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="2966d-441">Ubicaciones del módulo, el esquema y el archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="2966d-441">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="2966d-442">Module</span><span class="sxs-lookup"><span data-stu-id="2966d-442">Module</span></span>

<span data-ttu-id="2966d-443">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2966d-443">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="2966d-444">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-444">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="2966d-445">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-445">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2966d-446">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-446">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2966d-447">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-447">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="2966d-448">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="2966d-448">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="2966d-449">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-449">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="2966d-450">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-450">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2966d-451">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-451">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="2966d-452">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="2966d-452">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="2966d-453">Schema</span><span class="sxs-lookup"><span data-stu-id="2966d-453">Schema</span></span>

<span data-ttu-id="2966d-454">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2966d-454">**IIS**</span></span>

   * <span data-ttu-id="2966d-455">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2966d-455">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2966d-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2966d-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="2966d-457">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2966d-457">**IIS Express**</span></span>

   * <span data-ttu-id="2966d-458">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="2966d-458">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="2966d-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="2966d-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="2966d-460">Configuración</span><span class="sxs-lookup"><span data-stu-id="2966d-460">Configuration</span></span>

<span data-ttu-id="2966d-461">**IIS**</span><span class="sxs-lookup"><span data-stu-id="2966d-461">**IIS**</span></span>

   * <span data-ttu-id="2966d-462">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2966d-462">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="2966d-463">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2966d-463">**IIS Express**</span></span>

   * <span data-ttu-id="2966d-464">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="2966d-464">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="2966d-465">Los archivos se pueden encontrar mediante la búsqueda de *aspnetcore* en el archivo *applicationHost.config*.</span><span class="sxs-lookup"><span data-stu-id="2966d-465">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2966d-466">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2966d-466">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="2966d-467">Repositorio GitHub del módulo ASP.NET Core (origen de referencia)</span><span class="sxs-lookup"><span data-stu-id="2966d-467">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>