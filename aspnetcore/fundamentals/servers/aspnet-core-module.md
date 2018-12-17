---
title: Módulo ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo el módulo de ASP.NET Core permite que el servidor web de Kestrel use IIS o IIS Express.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861464"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="0a88a-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a88a-103">ASP.NET Core Module</span></span>

<span data-ttu-id="0a88a-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="0a88a-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0a88a-105">El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse en un proceso de trabajo de IIS (*en proceso*) o tras IIS en una configuración de proxy inverso (*fuera de proceso*).</span><span class="sxs-lookup"><span data-stu-id="0a88a-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="0a88a-106">IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.</span><span class="sxs-lookup"><span data-stu-id="0a88a-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0a88a-107">El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse tras IIS en una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="0a88a-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="0a88a-108">IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.</span><span class="sxs-lookup"><span data-stu-id="0a88a-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="0a88a-109">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="0a88a-109">Supported Windows versions:</span></span>

* <span data-ttu-id="0a88a-110">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="0a88a-110">Windows 7 or later</span></span>
* <span data-ttu-id="0a88a-111">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="0a88a-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0a88a-112">Cuando se hospeda en proceso, el módulo usa IIS HTTP Server (`IISHttpServer`), una implementación de servidor en proceso IIS.</span><span class="sxs-lookup"><span data-stu-id="0a88a-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="0a88a-113">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="0a88a-114">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="0a88a-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0a88a-115">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-115">The module only works with Kestrel.</span></span> <span data-ttu-id="0a88a-116">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="0a88a-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="0a88a-117">Descripción del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a88a-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0a88a-118">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="0a88a-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="0a88a-119">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="0a88a-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="0a88a-120">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="0a88a-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="0a88a-121">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="0a88a-121">In-process hosting model</span></span>

<span data-ttu-id="0a88a-122">Con el hospedaje en proceso, una aplicación ASP.NET Core se ejecuta en el mismo proceso que su proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="0a88a-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="0a88a-123">Esto evita la disminución del rendimiento que supone el envío de solicitudes mediante proxy a través del adaptador de bucle invertido con el modelo de hospedaje fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="0a88a-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="0a88a-124">IIS controla la administración de procesos con el [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="0a88a-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="0a88a-125">El módulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0a88a-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="0a88a-126">Realiza la inicialización de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="0a88a-126">Performs app initialization.</span></span>
  * <span data-ttu-id="0a88a-127">Carga [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="0a88a-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="0a88a-128">Llama a `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="0a88a-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="0a88a-129">Controla la duración de la solicitud nativa de IIS.</span><span class="sxs-lookup"><span data-stu-id="0a88a-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="0a88a-130">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada en proceso:</span><span class="sxs-lookup"><span data-stu-id="0a88a-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="0a88a-132">Una solicitud llega de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="0a88a-133">El controlador enruta la solicitud nativa a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0a88a-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="0a88a-134">El módulo recibe la solicitud nativa y la pasa a IIS HTTP Server (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="0a88a-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="0a88a-135">IIS HTTP Server es una implementación de servidor en proceso IIS que convierte una solicitud nativa en administrada.</span><span class="sxs-lookup"><span data-stu-id="0a88a-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="0a88a-136">Una vez que IIS HTTP Server procesa la solicitud, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a88a-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="0a88a-137">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0a88a-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="0a88a-138">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0a88a-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="0a88a-139">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="0a88a-139">Out-of-process hosting model</span></span>

<span data-ttu-id="0a88a-140">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="0a88a-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="0a88a-141">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea.</span><span class="sxs-lookup"><span data-stu-id="0a88a-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="0a88a-142">Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="0a88a-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="0a88a-143">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:</span><span class="sxs-lookup"><span data-stu-id="0a88a-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="0a88a-145">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="0a88a-146">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0a88a-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="0a88a-147">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="0a88a-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="0a88a-148">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="0a88a-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="0a88a-149">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="0a88a-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="0a88a-150">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0a88a-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="0a88a-151">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a88a-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="0a88a-152">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0a88a-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="0a88a-153">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="0a88a-154">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0a88a-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0a88a-155">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="0a88a-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="0a88a-156">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="0a88a-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="0a88a-157">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="0a88a-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="0a88a-158">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="0a88a-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="0a88a-159">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="0a88a-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="0a88a-161">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="0a88a-162">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0a88a-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="0a88a-163">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="0a88a-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="0a88a-164">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="0a88a-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="0a88a-165">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="0a88a-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="0a88a-166">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0a88a-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="0a88a-167">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a88a-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="0a88a-168">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0a88a-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="0a88a-169">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0a88a-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="0a88a-170">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0a88a-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="0a88a-171">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="0a88a-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="0a88a-172">Para obtener más información sobre los módulos de IIS activos con el módulo, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="0a88a-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="0a88a-173">El módulo ASP.NET Core tiene otras funciones.</span><span class="sxs-lookup"><span data-stu-id="0a88a-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="0a88a-174">Puede:</span><span class="sxs-lookup"><span data-stu-id="0a88a-174">The module can:</span></span>

* <span data-ttu-id="0a88a-175">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="0a88a-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="0a88a-176">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="0a88a-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="0a88a-177">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="0a88a-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="0a88a-178">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a88a-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="0a88a-179">Para obtener instrucciones detalladas sobre cómo instalar y usar el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="0a88a-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="0a88a-180">Para obtener información sobre cómo configurar el módulo, vea <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0a88a-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a88a-181">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0a88a-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="0a88a-182">Repositorio GitHub del módulo ASP.NET Core (código fuente)</span><span class="sxs-lookup"><span data-stu-id="0a88a-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
