---
title: Implementaciones de servidores web en ASP.NET Core
author: tdykstra
description: "Presenta los servidores web Kestrel y WebListener para ASP.NET Core. Proporciona instrucciones sobre cómo elegir uno y cuándo se debe usar con un servidor proxy inverso."
keywords: ASP.NET Core, IServer, servidor web, Kestrel, WebListener, proxy inverso
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: b149cb316e4266e67d846b8ef8c2c7f2a25ded5c
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="f189f-105">Implementaciones de servidores web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f189f-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="f189f-106">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="f189f-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="f189f-107">Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso.</span><span class="sxs-lookup"><span data-stu-id="f189f-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="f189f-108">La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como conjuntos de [características de solicitud](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) compuestos en un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="f189f-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="f189f-109">ASP.NET Core incluye dos implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="f189f-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f189f-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f189f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="f189f-111">[Kestrel](kestrel.md) es un servidor HTTP multiplataforma basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="f189f-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="f189f-112">[HTTP.sys](httpsys.md) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="f189f-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f189f-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f189f-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="f189f-114">[Kestrel](kestrel.md) es un servidor HTTP multiplataforma basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="f189f-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="f189f-115">[WebListener](weblistener.md) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="f189f-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="f189f-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-116">Kestrel</span></span>

<span data-ttu-id="f189f-117">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto nuevo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f189f-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f189f-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f189f-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f189f-119">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="f189f-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="f189f-120">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="f189f-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f189f-123">También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.</span><span class="sxs-lookup"><span data-stu-id="f189f-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="f189f-124">Para información sobre cuándo se debe usar Kestrel con un proxy inverso, vea [Introduction to Kestrel](kestrel.md) (Introducción a Kestrel).</span><span class="sxs-lookup"><span data-stu-id="f189f-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f189f-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f189f-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f189f-126">Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="f189f-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="f189f-128">Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="f189f-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="f189f-129">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar, como se muestra en el siguiente diagrama.</span><span class="sxs-lookup"><span data-stu-id="f189f-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="f189f-131">El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales (expuestas al tráfico de Internet) es la seguridad.</span><span class="sxs-lookup"><span data-stu-id="f189f-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="f189f-132">Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques.</span><span class="sxs-lookup"><span data-stu-id="f189f-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="f189f-133">Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño y los límites de conexiones simultáneas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="f189f-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="f189f-134">Para información sobre cuándo se debe usar Kestrel con un proxy inverso, vea [Introduction to Kestrel](kestrel.md) (Introducción a Kestrel).</span><span class="sxs-lookup"><span data-stu-id="f189f-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="f189f-135">No puede usar IIS, Nginx o Apache sin Kestrel ni ninguna [implementación de servidor personalizado](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="f189f-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="f189f-136">ASP.NET Core se ha diseñado para ejecutarse en su propio proceso de modo que pueda comportarse de forma coherente entre varias plataformas.</span><span class="sxs-lookup"><span data-stu-id="f189f-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="f189f-137">IIS, Nginx y Apache dictan su propio entorno y proceso de inicio; para poder usarlos directamente, ASP.NET Core tendría que adaptarse a las necesidades de cada uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="f189f-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="f189f-138">El uso de una implementación de servidor web como Kestrel proporciona a ASP.NET Core el control sobre el entorno y el proceso de inicio.</span><span class="sxs-lookup"><span data-stu-id="f189f-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="f189f-139">Por lo tanto, en lugar de intentar adaptar ASP.NET Core a IIS, Nginx o Apache, debe configurar esos servidores web en las solicitudes de proxy a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f189f-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="f189f-140">Esta disposición permite que las clases `Program.Main` y `Startup` sean básicamente las mismas, independientemente de dónde las implemente.</span><span class="sxs-lookup"><span data-stu-id="f189f-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="f189f-141">IIS con Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-141">IIS with Kestrel</span></span>

<span data-ttu-id="f189f-142">Al usar IIS o IIS Express como proxy inverso para ASP.NET Core, la aplicación ASP.NET Core se ejecuta en un proceso independiente del proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="f189f-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="f189f-143">En el proceso de IIS se ejecuta un módulo de IIS especial para coordinar la relación del proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="f189f-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="f189f-144">Se trata del *módulo de ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="f189f-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="f189f-145">Las funciones principales del módulo de ASP.NET Core consisten en iniciar la aplicación ASP.NET Core, reiniciarla cuando se bloquea y reenviar el tráfico HTTP a ella.</span><span class="sxs-lookup"><span data-stu-id="f189f-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="f189f-146">Para más información, vea [ASP.NET Core Module](aspnet-core-module.md) (Módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="f189f-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="f189f-147">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-147">Nginx with Kestrel</span></span>

<span data-ttu-id="f189f-148">Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx) (Hospedar en Linux con Nginx).</span><span class="sxs-lookup"><span data-stu-id="f189f-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="f189f-149">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-149">Apache with Kestrel</span></span>

<span data-ttu-id="f189f-150">Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Apache](xref:host-and-deploy/linux-apache) (Hospedar en Linux con Apache).</span><span class="sxs-lookup"><span data-stu-id="f189f-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="f189f-151">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f189f-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f189f-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f189f-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f189f-153">Si ejecuta la aplicación ASP.NET Core en Windows, HTTP.sys es una alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f189f-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="f189f-154">Puede usar HTTP.sys en los escenarios en los que expone la aplicación a Internet y necesita características de HTTP.sys con las que Kestrel no es compatible.</span><span class="sxs-lookup"><span data-stu-id="f189f-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="f189f-156">HTTP.sys también se puede usar para las aplicaciones que se exponen solo a una red interna.</span><span class="sxs-lookup"><span data-stu-id="f189f-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="f189f-158">Para los escenarios de red interna, se suele recomendar Kestrel para lograr un mejor rendimiento, pero en algunos escenarios es posible que quiera usar una característica que solo ofrece HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f189f-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="f189f-159">Para información sobre las características de HTTP.sys, vea [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="f189f-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f189f-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f189f-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f189f-161">HTTP.sys se denomina WebListener en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="f189f-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="f189f-162">Si ejecuta la aplicación ASP.NET Core en Windows, WebListener es una alternativa que puede usar para los escenarios en los que quiere exponer la aplicación a Internet, pero no puede usar IIS.</span><span class="sxs-lookup"><span data-stu-id="f189f-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="f189f-164">WebListener también se puede usar en lugar de Kestrel para las aplicaciones que se exponen solo a una red interna, en el caso de que necesite características de WebListener con las que Kestrel no es compatible.</span><span class="sxs-lookup"><span data-stu-id="f189f-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="f189f-166">Para los escenarios de red interna, se suele recomendar Kestrel para lograr un mejor rendimiento, pero en algunos escenarios es posible que quiera usar una característica que solo ofrece WebListener.</span><span class="sxs-lookup"><span data-stu-id="f189f-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="f189f-167">Para información sobre las características de WebListener, consulte [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="f189f-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="f189f-168">Notas sobre la infraestructura de servidores de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f189f-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="f189f-169">El [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible en el método `Configure` de la clase `Startup` expone la propiedad `ServerFeatures` de tipo [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="f189f-169">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="f189f-170">Kestrel y WebListener exponen una sola característica, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), pero las distintas implementaciones del servidor pueden exponer una funcionalidad adicional.</span><span class="sxs-lookup"><span data-stu-id="f189f-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="f189f-171">Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f189f-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="f189f-172">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="f189f-172">Custom servers</span></span>

<span data-ttu-id="f189f-173">Si los servidores integrados no satisfacen sus necesidades, puede crear una implementación de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="f189f-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="f189f-174">En la [guía de Open Web Interface for .NET (OWIN)](../owin.md) se muestra cómo escribir una implementación de [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) basada en [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="f189f-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="f189f-175">Tiene la posibilidad de implementar únicamente las interfaces de las características necesarias para la aplicación, aunque como mínimo debe admitir [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="f189f-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f189f-176">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f189f-176">Next steps</span></span>

<span data-ttu-id="f189f-177">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="f189f-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f189f-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f189f-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="f189f-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="f189f-180">Kestrel con IIS</span><span class="sxs-lookup"><span data-stu-id="f189f-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="f189f-181">Hospedaje en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="f189f-181">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="f189f-182">Hospedaje en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="f189f-182">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="f189f-183">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f189f-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f189f-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f189f-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="f189f-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="f189f-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="f189f-186">Kestrel con IIS</span><span class="sxs-lookup"><span data-stu-id="f189f-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="f189f-187">Hospedaje en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="f189f-187">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="f189f-188">Hospedaje en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="f189f-188">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="f189f-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="f189f-189">WebListener</span></span>](weblistener.md)

---
