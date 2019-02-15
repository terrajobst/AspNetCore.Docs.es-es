---
title: Implementaciones de servidores web en ASP.NET Core
author: guardrex
description: Detecte los servidores web Kestrel y HTTP.sys de ASP.NET Core. Obtenga más información sobre cómo elegir un servidor y cuándo se debe usar un servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: fundamentals/servers/index
ms.openlocfilehash: 672fe2ce6fd0adae09c380fe508344a254f1a9fe
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248139"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="61603-104">Implementaciones de servidores web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61603-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="61603-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="61603-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="61603-106">Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso.</span><span class="sxs-lookup"><span data-stu-id="61603-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="61603-107">La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) compuestos en un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="61603-107">The server implementation listens for HTTP requests and surfaces them to the app as a set of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="61603-108">Windows</span><span class="sxs-lookup"><span data-stu-id="61603-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="61603-109">ASP.NET Core se suministra con los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="61603-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="61603-110">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es la implementación de servidor HTTP multiplataforma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="61603-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server implementation.</span></span>
* <span data-ttu-id="61603-111">El servidor HTTP de IIS es un [servidor en proceso](#in-process-hosting-model) de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-111">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="61603-112">El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="61603-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="61603-113">Cuando se usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="61603-113">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app either runs:</span></span>

* <span data-ttu-id="61603-114">En el mismo proceso que el proceso de trabajo de IIS (el [modelo de hospedaje en proceso](#in-process-hosting-model)) con el [servidor HTTP de IIS](#iis-http-server).</span><span class="sxs-lookup"><span data-stu-id="61603-114">In the same process as the IIS worker process (the [in-process hosting model](#in-process-hosting-model)) with the [IIS HTTP Server](#iis-http-server).</span></span> <span data-ttu-id="61603-115">La configuración recomendada es *En proceso*.</span><span class="sxs-lookup"><span data-stu-id="61603-115">*In-process* is the recommended configuration.</span></span>
* <span data-ttu-id="61603-116">En un proceso distinto al del proceso de trabajo de IIS (el [modelo de hospedaje fuera de proceso](#out-of-process-hosting-model)) con el [servidor Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="61603-116">In a process separate from the IIS worker process (the [out-of-process hosting model](#out-of-process-hosting-model)) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="61603-117">El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) es un módulo nativo de IIS que controla las solicitudes de IIS nativas entre IIS y el servidor de IIS en proceso o Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is a native IIS module that handles native IIS requests between IIS and the in-process IIS HTTP Server or Kestrel.</span></span> <span data-ttu-id="61603-118">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="61603-118">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="hosting-models"></a><span data-ttu-id="61603-119">Modelos de hospedaje</span><span class="sxs-lookup"><span data-stu-id="61603-119">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="61603-120">Modelo de hospedaje en proceso</span><span class="sxs-lookup"><span data-stu-id="61603-120">In-process hosting model</span></span>

<span data-ttu-id="61603-121">Con el hospedaje en proceso, una aplicación ASP.NET Core se ejecuta en el mismo proceso que su proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-121">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="61603-122">El hospedaje en proceso proporciona un rendimiento mejorado con respecto al hospedaje fuera de proceso porque las solicitudes no se realizan mediante proxy en el adaptador de bucle invertido, una interfaz de red que devuelve el tráfico saliente a la misma máquina.</span><span class="sxs-lookup"><span data-stu-id="61603-122">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="61603-123">IIS controla la administración de procesos con el [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="61603-123">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="61603-124">El módulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="61603-124">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="61603-125">Realiza la inicialización de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="61603-125">Performs app initialization.</span></span>
  * <span data-ttu-id="61603-126">Carga [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="61603-126">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="61603-127">Llama a `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="61603-127">Calls `Program.Main`.</span></span>
* <span data-ttu-id="61603-128">Controla la duración de la solicitud nativa de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-128">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="61603-129">No se admite el modelo de hospedaje en proceso para aplicaciones de ASP.NET Core que tienen como destino .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="61603-129">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="61603-130">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada en proceso:</span><span class="sxs-lookup"><span data-stu-id="61603-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Módulo ASP.NET Core](_static/ancm-inprocess.png)

<span data-ttu-id="61603-132">Una solicitud llega de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="61603-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="61603-133">El controlador enruta la solicitud nativa a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61603-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="61603-134">El módulo recibe la solicitud nativa y la pasa a IIS HTTP Server (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="61603-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="61603-135">El servidor HTTP de IIS es una implementación de servidor en proceso para IIS que convierte una solicitud nativa en administrada.</span><span class="sxs-lookup"><span data-stu-id="61603-135">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="61603-136">Una vez que IIS HTTP Server procesa la solicitud, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61603-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="61603-137">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61603-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="61603-138">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61603-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

<span data-ttu-id="61603-139">El hospedaje en proceso es opcional para las aplicaciones existentes, pero, para las plantillas [dotnet new](/dotnet/core/tools/dotnet-new), este modelo es el predeterminado para todos los escenarios de IIS e IIS Express.</span><span class="sxs-lookup"><span data-stu-id="61603-139">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="61603-140">Modelo de hospedaje fuera de proceso</span><span class="sxs-lookup"><span data-stu-id="61603-140">Out-of-process hosting model</span></span>

<span data-ttu-id="61603-141">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="61603-141">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="61603-142">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea.</span><span class="sxs-lookup"><span data-stu-id="61603-142">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="61603-143">Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="61603-143">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="61603-144">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:</span><span class="sxs-lookup"><span data-stu-id="61603-144">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Módulo ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="61603-146">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="61603-146">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="61603-147">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61603-147">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="61603-148">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="61603-148">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="61603-149">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{PORT}`.</span><span class="sxs-lookup"><span data-stu-id="61603-149">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="61603-150">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="61603-150">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="61603-151">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61603-151">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="61603-152">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61603-152">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="61603-153">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61603-153">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="61603-154">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-154">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="61603-155">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61603-155">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="61603-156">Para obtener instrucciones de configuración para IIS y el módulo ASP.NET Core, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="61603-156">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="61603-157">macOS</span><span class="sxs-lookup"><span data-stu-id="61603-157">macOS</span></span>](#tab/macos)

<span data-ttu-id="61603-158">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61603-158">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="61603-159">Linux</span><span class="sxs-lookup"><span data-stu-id="61603-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="61603-160">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61603-160">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="61603-161">Windows</span><span class="sxs-lookup"><span data-stu-id="61603-161">Windows</span></span>](#tab/windows)

<span data-ttu-id="61603-162">ASP.NET Core se suministra con los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="61603-162">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="61603-163">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61603-163">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="61603-164">El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](/windows/desktop/Http/http-api-start-page).</span><span class="sxs-lookup"><span data-stu-id="61603-164">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](/windows/desktop/Http/http-api-start-page).</span></span>

<span data-ttu-id="61603-165">Al usar [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación se ejecuta en un proceso distinto al del proceso de trabajo de IIS (*fuera de proceso*) con el [servidor Kestrel](#kestrel).</span><span class="sxs-lookup"><span data-stu-id="61603-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](#kestrel).</span></span>

<span data-ttu-id="61603-166">Dado que las aplicaciones ASP.NET Core se ejecutan en un proceso independiente del proceso de trabajo de IIS, el módulo se encarga de la administración de procesos.</span><span class="sxs-lookup"><span data-stu-id="61603-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="61603-167">El módulo inicia el proceso de la aplicación ASP.NET Core cuando entra la primera solicitud y reinicia la aplicación si esta se apaga o se bloquea.</span><span class="sxs-lookup"><span data-stu-id="61603-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="61603-168">Este comportamiento es básicamente el mismo que el de las aplicaciones que se ejecutan en proceso y se administran a través del [Servicio de activación de procesos de Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="61603-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="61603-169">En el siguiente diagrama se muestra la relación entre IIS, el módulo ASP.NET Core y una aplicación hospedada fuera de proceso:</span><span class="sxs-lookup"><span data-stu-id="61603-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Módulo ASP.NET Core](_static/ancm-outofprocess.png)

<span data-ttu-id="61603-171">Las solicitudes llegan procedentes de Internet al controlador HTTP.sys en modo kernel.</span><span class="sxs-lookup"><span data-stu-id="61603-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="61603-172">El controlador enruta las solicitudes a IIS en el puerto configurado del sitio web, que suele ser el puerto 80 (HTTP) o 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="61603-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="61603-173">El módulo reenvía las solicitudes a Kestrel en un puerto aleatorio de la aplicación, que no es ni 80 ni 443.</span><span class="sxs-lookup"><span data-stu-id="61603-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="61603-174">El módulo especifica el puerto a través de la variable de entorno en el inicio y el middleware de integración de IIS configura el servidor para que escuche en `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="61603-174">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="61603-175">Se realizan más comprobaciones y se rechazan las solicitudes que no se hayan originado en el módulo.</span><span class="sxs-lookup"><span data-stu-id="61603-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="61603-176">El módulo no admite el reenvío de HTTPS, por lo que las solicitudes se reenvían a través de HTTP, aunque IIS las reciba a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61603-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="61603-177">Una vez que Kestrel toma la solicitud del módulo, la envía a la canalización de middleware de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61603-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="61603-178">La canalización de middleware controla la solicitud y la pasa como una instancia de `HttpContext` a la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61603-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="61603-179">El middleware agregado por la integración de IIS actualiza el esquema, la dirección IP remota y PathBase para responder del reenvío de la solicitud a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="61603-180">La respuesta de la aplicación se vuelve a pasar a IIS, que la envía de nuevo al cliente HTTP que inició la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61603-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="61603-181">Para obtener instrucciones de configuración para IIS y el módulo ASP.NET Core, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="61603-181">For IIS and ASP.NET Core Module configuration guidance, see the following topics:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>

# <a name="macostabmacos"></a>[<span data-ttu-id="61603-182">macOS</span><span class="sxs-lookup"><span data-stu-id="61603-182">macOS</span></span>](#tab/macos)

<span data-ttu-id="61603-183">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61603-183">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="61603-184">Linux</span><span class="sxs-lookup"><span data-stu-id="61603-184">Linux</span></span>](#tab/linux)

<span data-ttu-id="61603-185">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61603-185">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="61603-186">Kestrel</span><span class="sxs-lookup"><span data-stu-id="61603-186">Kestrel</span></span>

<span data-ttu-id="61603-187">Kestrel es el servidor web predeterminado que se incluye en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61603-187">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="61603-188">Kestrel se puede usar:</span><span class="sxs-lookup"><span data-stu-id="61603-188">Kestrel can be used:</span></span>

* <span data-ttu-id="61603-189">Por sí solo como un servidor perimetral que procesa solicitudes directamente desde una red, incluida Internet.</span><span class="sxs-lookup"><span data-stu-id="61603-189">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>

  ![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

* <span data-ttu-id="61603-191">Con un *servidor proxy inverso*, tal como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="61603-191">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="61603-192">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-192">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

  ![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="61603-194">Se admite cualquiera de las configuraciones de hospedaje, &mdash; con o sin un servidor proxy inverso&mdash; para ASP.NET Core 2.1 o aplicaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="61603-194">Either hosting configuration&mdash;with or without a reverse proxy server&mdash;is supported for ASP.NET Core 2.1 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="61603-195">Si la aplicación únicamente acepta solicitudes de una red interna, Kestrel puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="61603-195">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="61603-197">Si la aplicación se expone a Internet, Kestrel debe usar un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache ](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="61603-197">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="61603-198">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-198">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="61603-200">El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales de acceso público expuestos directamente a Internet es la seguridad.</span><span class="sxs-lookup"><span data-stu-id="61603-200">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="61603-201">Las versiones 1.x de Kestrel no incluyen características de seguridad importantes para defenderse contra ataques procedentes de Internet.</span><span class="sxs-lookup"><span data-stu-id="61603-201">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="61603-202">Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño de solicitud y los límites de conexiones simultáneas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="61603-202">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="61603-203">Si quiere obtener instrucciones e información sobre la configuración de Kestrel y su uso en una configuración de proxy inverso, vea <xref:fundamentals/servers/kestrel>.</span><span class="sxs-lookup"><span data-stu-id="61603-203">For Kestrel configuration guidance and information on when to use Kestrel in a reverse proxy configuration, see <xref:fundamentals/servers/kestrel>.</span></span>

### <a name="nginx-with-kestrel"></a><span data-ttu-id="61603-204">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="61603-204">Nginx with Kestrel</span></span>

<span data-ttu-id="61603-205">Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="61603-205">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="61603-206">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="61603-206">Apache with Kestrel</span></span>

<span data-ttu-id="61603-207">Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="61603-207">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="iis-http-server"></a><span data-ttu-id="61603-208">Servidor HTTP de IIS</span><span class="sxs-lookup"><span data-stu-id="61603-208">IIS HTTP Server</span></span>

<span data-ttu-id="61603-209">El servidor HTTP de IIS es un [servidor en proceso](#in-process-hosting-model) para IIS necesario para las implementaciones en proceso.</span><span class="sxs-lookup"><span data-stu-id="61603-209">IIS HTTP Server is an [in-process server](#in-process-hosting-model) for IIS and required for in-process deployments.</span></span> <span data-ttu-id="61603-210">El [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) controla las solicitudes de IIS nativas entre IIS y el servidor HTTP de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-210">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) handles native IIS requests between IIS and IIS HTTP Server.</span></span> <span data-ttu-id="61603-211">Para obtener más información, vea <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="61603-211">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="61603-212">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="61603-212">HTTP.sys</span></span>

<span data-ttu-id="61603-213">Si las aplicaciones ASP.NET Core se ejecutan en Windows, HTTP.sys es una alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-213">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="61603-214">Suele recomendarse Kestrel para un rendimiento óptimo.</span><span class="sxs-lookup"><span data-stu-id="61603-214">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="61603-215">HTTP.sys se puede usar en escenarios en los que la aplicación se expone a Internet y las funcionalidades necesarias son compatibles con HTTP.sys pero no con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="61603-215">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="61603-216">Para obtener más información, vea <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="61603-216">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="61603-218">HTTP.sys también se puede usar para las aplicaciones que solo se exponen a una red interna.</span><span class="sxs-lookup"><span data-stu-id="61603-218">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="61603-220">Para obtener instrucciones de configuración de HTTP.sys, vea <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="61603-220">For HTTP.sys configuration guidance, see <xref:fundamentals/servers/httpsys>.</span></span>

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="61603-221">Infraestructura de servidores de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61603-221">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="61603-222">El elemento <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> disponible en el método `Startup.Configure` expone la propiedad <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> del tipo <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span><span class="sxs-lookup"><span data-stu-id="61603-222">The <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> available in the `Startup.Configure` method exposes the <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ServerFeatures> property of type <xref:Microsoft.AspNetCore.Http.Features.IFeatureCollection>.</span></span> <span data-ttu-id="61603-223">Kestrel y HTTP.sys solo exponen una característica cada uno (<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>), pero otras implementaciones de servidor pueden exponer funcionalidades adicionales.</span><span class="sxs-lookup"><span data-stu-id="61603-223">Kestrel and HTTP.sys only expose a single feature each, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>, but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="61603-224">Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="61603-224">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="61603-225">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="61603-225">Custom servers</span></span>

<span data-ttu-id="61603-226">Si los servidores integrados no cumplen los requisitos de la aplicación, se puede crear una implementación de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="61603-226">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="61603-227">En la [guía de Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) se muestra cómo escribir una implementación de <xref:Microsoft.AspNetCore.Hosting.Server.IServer> basada en [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="61603-227">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation.</span></span> <span data-ttu-id="61603-228">Solo las interfaces de la característica que usa la aplicación requieren implementación, aunque como mínimo se debe admitir <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> y <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature>.</span><span class="sxs-lookup"><span data-stu-id="61603-228">Only the feature interfaces that the app uses require implementation, though at a minimum <xref:Microsoft.AspNetCore.Http.Features.IHttpRequestFeature> and <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature> must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="61603-229">Inicio del servidor</span><span class="sxs-lookup"><span data-stu-id="61603-229">Server startup</span></span>

<span data-ttu-id="61603-230">El servidor se inicia cuando el entorno de desarrollo integrado (IDE) o editor inicia la aplicación:</span><span class="sxs-lookup"><span data-stu-id="61603-230">The server is launched when the Integrated Development Environment (IDE) or editor starts the app:</span></span>

* <span data-ttu-id="61603-231">[Visual Studio](https://www.visualstudio.com/vs/): los perfiles de inicio se pueden usar para iniciar la aplicación y el servidor con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module) o la consola.</span><span class="sxs-lookup"><span data-stu-id="61603-231">[Visual Studio](https://www.visualstudio.com/vs/) &ndash; Launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) or the console.</span></span>
* <span data-ttu-id="61603-232">[Visual Studio Code](https://code.visualstudio.com/): la aplicación y el servidor se inician mediante [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), con lo que se activa el depurador CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="61603-232">[Visual Studio Code](https://code.visualstudio.com/) &ndash; The app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span>
* <span data-ttu-id="61603-233">[Visual Studio para Mac](https://www.visualstudio.com/vs/mac/): la aplicación y el servidor se inician mediante [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="61603-233">[Visual Studio for Mac](https://www.visualstudio.com/vs/mac/) &ndash; The app and server are started by the [Mono Soft-Mode Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="61603-234">Al iniciar la aplicación desde un símbolo del sistema en la carpeta del proyecto, [dotnet run](/dotnet/core/tools/dotnet-run) inicia la aplicación y el servidor (solo Kestrel y HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="61603-234">When launching the app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="61603-235">La configuración se especifica mediante la opción `-c|--configuration`, que está establecida en `Debug` (valor predeterminado) o `Release`.</span><span class="sxs-lookup"><span data-stu-id="61603-235">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="61603-236">Si no hay perfiles de inicio en un archivo *launchSettings.json*, use la opción `--launch-profile <NAME>` para establecer el perfil de inicio (por ejemplo, `Development` o `Production`).</span><span class="sxs-lookup"><span data-stu-id="61603-236">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="61603-237">Para más información, vea [dotnet run](/dotnet/core/tools/dotnet-run) y [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="61603-237">For more information, see [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

## <a name="http2-support"></a><span data-ttu-id="61603-238">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="61603-238">HTTP/2 support</span></span>

<span data-ttu-id="61603-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación siguientes:</span><span class="sxs-lookup"><span data-stu-id="61603-239">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="61603-240">Kestrel</span><span class="sxs-lookup"><span data-stu-id="61603-240">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="61603-241">Sistema operativo</span><span class="sxs-lookup"><span data-stu-id="61603-241">Operating system</span></span>
    * <span data-ttu-id="61603-242">Windows Server 2016/Windows 10 o posterior&dagger;</span><span class="sxs-lookup"><span data-stu-id="61603-242">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="61603-243">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="61603-243">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="61603-244">HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="61603-244">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="61603-245">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-245">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="61603-246">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="61603-246">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="61603-247">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-247">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="61603-248">Marco de destino: No aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="61603-248">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="61603-249">IIS (en proceso)</span><span class="sxs-lookup"><span data-stu-id="61603-249">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="61603-250">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-250">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="61603-251">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-251">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="61603-252">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="61603-252">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="61603-253">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-253">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="61603-254">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61603-254">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="61603-255">Marco de destino: No aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-255">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="61603-256">&dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="61603-256">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="61603-257">La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada.</span><span class="sxs-lookup"><span data-stu-id="61603-257">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="61603-258">Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.</span><span class="sxs-lookup"><span data-stu-id="61603-258">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="61603-259">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="61603-259">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="61603-260">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-260">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="61603-261">Marco de destino: No aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="61603-261">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="61603-262">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="61603-262">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="61603-263">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="61603-263">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="61603-264">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="61603-264">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="61603-265">Marco de destino: No aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="61603-265">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="61603-266">Una conexión HTTP/2 debe usar [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) y TLS 1.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="61603-266">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="61603-267">Para más información, vea los temas que pertenecen a los escenarios de implementación de servidor.</span><span class="sxs-lookup"><span data-stu-id="61603-267">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61603-268">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="61603-268">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys>
