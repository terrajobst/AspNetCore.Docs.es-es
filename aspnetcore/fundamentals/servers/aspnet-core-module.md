---
title: Módulo ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo el módulo de ASP.NET Core permite que el servidor web de Kestrel use IIS o IIS Express como servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910894"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="955c8-103">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="955c8-103">ASP.NET Core Module</span></span>

<span data-ttu-id="955c8-104">Por [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="955c8-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="955c8-105">El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse en un proceso de trabajo de IIS (*en proceso*) o tras IIS en una configuración de proxy inverso (*fuera de proceso*).</span><span class="sxs-lookup"><span data-stu-id="955c8-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="955c8-106">IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.</span><span class="sxs-lookup"><span data-stu-id="955c8-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="955c8-107">El módulo ASP.NET Core permite a las aplicaciones ASP.NET Core ejecutarse tras IIS en una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="955c8-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="955c8-108">IIS proporciona características de administración y seguridad de aplicaciones web avanzadas.</span><span class="sxs-lookup"><span data-stu-id="955c8-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="955c8-109">Versiones de Windows compatibles:</span><span class="sxs-lookup"><span data-stu-id="955c8-109">Supported Windows versions:</span></span>

* <span data-ttu-id="955c8-110">Windows 7 o posterior</span><span class="sxs-lookup"><span data-stu-id="955c8-110">Windows 7 or later</span></span>
* <span data-ttu-id="955c8-111">Windows Server 2008 R2 o posterior</span><span class="sxs-lookup"><span data-stu-id="955c8-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="955c8-112">Cuando se hospeda en proceso, el módulo tiene su propia implementación de servidor, `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="955c8-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="955c8-113">Cuando se hospeda fuera de proceso, el módulo solo funciona con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="955c8-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="955c8-114">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="955c8-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="955c8-115">El módulo funciona únicamente con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="955c8-115">The module only works with Kestrel.</span></span> <span data-ttu-id="955c8-116">El módulo no es compatible con [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="955c8-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="955c8-117">Descripción del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="955c8-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="955c8-118">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para:</span><span class="sxs-lookup"><span data-stu-id="955c8-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="955c8-119">Hospedar una aplicación ASP.NET Core dentro del proceso de trabajo de IIS (`w3wp.exe`), lo que se denomina [modelo de hospedaje en proceso](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="955c8-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="955c8-120">Reenviar solicitudes web a una aplicación ASP.NET Core de back-end que ejecuta el [servidor de Kestrel](xref:fundamentals/servers/kestrel), lo que se denomina [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="955c8-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="955c8-121">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="955c8-121">In-process hosting model</span></span>

<span data-ttu-id="955c8-122">Con el hospedaje en proceso, una aplicación ASP.NET Core se ejecuta en el mismo proceso que su proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="955c8-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="955c8-123">Esto evita la disminución del rendimiento que supone el envío de solicitudes mediante proxy a través del adaptador de bucle invertido con el modelo de hospedaje fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="955c8-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="955c8-124">IIS controla la administración de procesos con el [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="955c8-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="955c8-125">El módulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="955c8-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="955c8-126">Realiza la inicialización de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="955c8-126">Performs app initialization.</span></span>
  * <span data-ttu-id="955c8-127">Carga [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="955c8-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="955c8-128">Llama a `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="955c8-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="955c8-129">Controla la duración de la solicitud nativa de IIS.</span><span class="sxs-lookup"><span data-stu-id="955c8-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="955c8-130">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada en proceso:</span><span class="sxs-lookup"><span data-stu-id="955c8-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="955c8-132">Una solicitud llega de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="955c8-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="955c8-133">El controlador enruta la solicitud nativa a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="955c8-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="955c8-134">El módulo recibe la solicitud nativa y pasa el control a `IISHttpServer`, que es lo que convierte la solicitud de nativa a administrada.</span><span class="sxs-lookup"><span data-stu-id="955c8-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="955c8-135">Una vez que `IISHttpServer` toma la solicitud, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="955c8-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="955c8-136">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="955c8-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="955c8-137">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="955c8-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="955c8-138">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="955c8-138">Out-of-process hosting model</span></span>

<span data-ttu-id="955c8-139">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="955c8-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="955c8-140">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea.</span><span class="sxs-lookup"><span data-stu-id="955c8-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="955c8-141">Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="955c8-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="955c8-142">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:</span><span class="sxs-lookup"><span data-stu-id="955c8-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="955c8-144">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="955c8-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="955c8-145">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="955c8-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="955c8-146">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="955c8-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="955c8-147">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="955c8-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="955c8-148">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="955c8-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="955c8-149">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="955c8-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="955c8-150">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="955c8-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="955c8-151">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="955c8-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="955c8-152">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="955c8-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="955c8-153">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="955c8-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="955c8-154">El módulo ASP.NET Core es un módulo nativo de IIS que se conecta a la canalización de IIS para reenviar solicitudes web a aplicaciones ASP.NET Core de back-end.</span><span class="sxs-lookup"><span data-stu-id="955c8-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="955c8-155">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo también se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="955c8-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="955c8-156">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud, y reinicia la aplicación si esta se bloquea.</span><span class="sxs-lookup"><span data-stu-id="955c8-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="955c8-157">Este comportamiento es básicamente el mismo que el de las aplicaciones ASP.NET 4.x que se ejecutan en el proceso en IIS y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="955c8-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="955c8-158">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación:</span><span class="sxs-lookup"><span data-stu-id="955c8-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Módulo ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="955c8-160">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="955c8-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="955c8-161">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="955c8-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="955c8-162">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="955c8-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="955c8-163">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="955c8-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="955c8-164">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="955c8-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="955c8-165">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="955c8-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="955c8-166">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="955c8-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="955c8-167">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="955c8-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="955c8-168">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="955c8-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="955c8-169">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="955c8-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="955c8-170">Muchos de los módulos nativos, como la autenticación de Windows, permanecen activos.</span><span class="sxs-lookup"><span data-stu-id="955c8-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="955c8-171">Para obtener más información sobre los módulos de IIS activos con el módulo, vea <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="955c8-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="955c8-172">El módulo ASP.NET Core tiene otras funciones.</span><span class="sxs-lookup"><span data-stu-id="955c8-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="955c8-173">Puede:</span><span class="sxs-lookup"><span data-stu-id="955c8-173">The module can:</span></span>

* <span data-ttu-id="955c8-174">Establecer variables de entorno para un proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="955c8-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="955c8-175">Registrar la salida en un almacenamiento de archivos para solucionar problemas de inicio.</span><span class="sxs-lookup"><span data-stu-id="955c8-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="955c8-176">Reenviar tokens de autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="955c8-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="955c8-177">Cómo instalar y usar el módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="955c8-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="955c8-178">Para obtener instrucciones detalladas sobre cómo instalar y usar el módulo ASP.NET Core, vea <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="955c8-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="955c8-179">Para obtener información sobre cómo configurar el módulo, vea <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="955c8-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="955c8-180">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="955c8-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="955c8-181">Repositorio GitHub del módulo ASP.NET Core (código fuente)</span><span class="sxs-lookup"><span data-stu-id="955c8-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
