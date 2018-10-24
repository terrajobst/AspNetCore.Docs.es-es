---
title: Implementaciones de servidores web en ASP.NET Core
author: rick-anderson
description: Detecte los servidores web Kestrel y HTTP.sys de ASP.NET Core. Obtenga más información sobre cómo elegir un servidor y cuándo se debe usar un servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 161ab3fdf48e58d8c9af991dc5531e46d9c5adff
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325866"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="ea607-104">Implementaciones de servidores web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea607-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="ea607-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="ea607-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="ea607-106">Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso.</span><span class="sxs-lookup"><span data-stu-id="ea607-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="ea607-107">La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como conjuntos de [características de solicitud](xref:fundamentals/request-features) compuestos en un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="ea607-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="ea607-108">ASP.NET Core incluye tres implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="ea607-108">ASP.NET Core ships three server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ea607-109">[Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea607-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="ea607-110">`IISHttpServer` se usa con el [modelo de hospedaje en proceso](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) en Windows.</span><span class="sxs-lookup"><span data-stu-id="ea607-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="ea607-111">[HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de HTTP.Sys y HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea607-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ea607-112">(HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).</span><span class="sxs-lookup"><span data-stu-id="ea607-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ea607-113">[Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea607-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="ea607-114">[HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo para Windows que se basa en el [controlador de kernel de HTTP.Sys y HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea607-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="ea607-115">(HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).</span><span class="sxs-lookup"><span data-stu-id="ea607-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="ea607-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ea607-116">Kestrel</span></span>

<span data-ttu-id="ea607-117">Kestrel es el servidor web predeterminado que se incluye en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea607-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ea607-118">Se puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="ea607-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="ea607-119">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="ea607-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ea607-122">Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="ea607-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="ea607-123">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="ea607-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ea607-124">Si la aplicación únicamente acepta solicitudes de una red interna, Kestrel puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="ea607-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="ea607-126">Si la aplicación se expone a Internet, Kestrel debe usar IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="ea607-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="ea607-127">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar, como se muestra en el diagrama siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea607-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="ea607-129">El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales de acceso público expuestos directamente a Internet es la seguridad.</span><span class="sxs-lookup"><span data-stu-id="ea607-129">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="ea607-130">Las versiones 1.x de Kestrel no tienen características de seguridad importantes para defenderse contra ataques procedentes de Internet.</span><span class="sxs-lookup"><span data-stu-id="ea607-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="ea607-131">Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño de solicitud y los límites de conexiones simultáneas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="ea607-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="ea607-132">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="ea607-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="ea607-133">No se puede usar IIS, Nginx o Apache sin Kestrel ni ninguna [implementación de servidor personalizado](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="ea607-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="ea607-134">ASP.NET Core se ha diseñado para ejecutarse en su propio proceso de modo que pueda comportarse de forma coherente entre varias plataformas.</span><span class="sxs-lookup"><span data-stu-id="ea607-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="ea607-135">IIS, Nginx y Apache dictan su propio procedimiento y entorno de inicio.</span><span class="sxs-lookup"><span data-stu-id="ea607-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="ea607-136">Para usar estas tecnologías de servidor directamente, ASP.NET Core tendría que adaptarse a los requisitos de cada servidor.</span><span class="sxs-lookup"><span data-stu-id="ea607-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="ea607-137">El uso de una implementación de servidor web como Kestrel proporciona a ASP.NET Core el control sobre el entorno y el proceso de inicio cuando se hospedan en tecnologías de servidor diferentes.</span><span class="sxs-lookup"><span data-stu-id="ea607-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="ea607-138">IIS con Kestrel</span><span class="sxs-lookup"><span data-stu-id="ea607-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea607-139">Cuando se usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación de ASP.NET Core se ejecuta en el mismo proceso que el proceso de trabajo de IIS (el modelo de hospedaje *en proceso*) o en un proceso independiente del proceso de trabajo de IIS (el modelo de hospedaje *fuera de proceso*).</span><span class="sxs-lookup"><span data-stu-id="ea607-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="ea607-140">El [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) es un módulo nativo de IIS que controla las solicitudes de IIS nativas entre el servidor de HTTP de IIS en proceso o el servidor Kestrel fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="ea607-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="ea607-141">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ea607-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ea607-142">Al usar [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como proxy inverso para ASP.NET Core, la aplicación ASP.NET Core se ejecuta en un proceso independiente del proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="ea607-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="ea607-143">En el proceso IIS, el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordina la relación de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="ea607-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="ea607-144">Las funciones principales del módulo de ASP.NET Core consisten en iniciar la aplicación ASP.NET Core, reiniciarla cuando se bloquea y reenviar el tráfico HTTP a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea607-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="ea607-145">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="ea607-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="ea607-146">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="ea607-146">Nginx with Kestrel</span></span>

<span data-ttu-id="ea607-147">Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx) (Hospedar en Linux con Nginx).</span><span class="sxs-lookup"><span data-stu-id="ea607-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="ea607-148">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="ea607-148">Apache with Kestrel</span></span>

<span data-ttu-id="ea607-149">Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, vea [Host on Linux with Apache](xref:host-and-deploy/linux-apache) (Hospedar en Linux con Apache).</span><span class="sxs-lookup"><span data-stu-id="ea607-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="ea607-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ea607-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ea607-151">Si las aplicaciones ASP.NET Core se ejecutan en Windows, HTTP.sys es una alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea607-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="ea607-152">Suele recomendarse Kestrel para un rendimiento óptimo.</span><span class="sxs-lookup"><span data-stu-id="ea607-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="ea607-153">HTTP.sys se puede usar en escenarios en los que la aplicación se expone a Internet y las funcionalidades necesarias son compatibles con HTTP.sys pero no con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea607-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="ea607-154">Para información sobre HTTP.sys, vea [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="ea607-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="ea607-156">HTTP.sys también se puede usar para las aplicaciones que solo se exponen a una red interna.</span><span class="sxs-lookup"><span data-stu-id="ea607-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ea607-158">HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ea607-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="ea607-159">Si las aplicaciones ASP.NET Core se ejecutan en Windows, WebListener es una alternativa para los escenarios en los que IIS no está disponible para hospedar aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ea607-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="ea607-161">WebListener también se puede usar en lugar de Kestrel para las aplicaciones que solo se exponen a una red interna, en el caso de que las funcionalidades necesarias sean compatibles con WebListener pero no con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ea607-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="ea607-162">Para información sobre WebListener, vea [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="ea607-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="ea607-164">Infraestructura de servidores de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea607-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="ea607-165">La interfaz [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible en el método `Startup.Configure` expone la propiedad [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="ea607-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="ea607-166">Kestrel y HTTP.sys (WebListener en ASP.NET Core 1.x) solo exponen una característica cada uno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), pero otras implementaciones de servidor pueden exponer funcionalidades adicionales.</span><span class="sxs-lookup"><span data-stu-id="ea607-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="ea607-167">Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ea607-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="ea607-168">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="ea607-168">Custom servers</span></span>

<span data-ttu-id="ea607-169">Si los servidores integrados no cumplen los requisitos de la aplicación, se puede crear una implementación de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="ea607-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="ea607-170">En la [guía de Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) se muestra cómo escribir una implementación de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basada en [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="ea607-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="ea607-171">Solo requieren la implementación las interfaces de características que usa la aplicación, aunque como mínimo se debe admitir [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="ea607-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="ea607-172">Inicio del servidor</span><span class="sxs-lookup"><span data-stu-id="ea607-172">Server startup</span></span>

<span data-ttu-id="ea607-173">Cuando se usa [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/), el servidor se inicia cuando el entorno de desarrollo integrado (IDE) inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea607-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="ea607-174">En Visual Studio en Windows, se pueden usar perfiles de inicio para iniciar la aplicación y el servidor con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) o la consola.</span><span class="sxs-lookup"><span data-stu-id="ea607-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="ea607-175">En Visual Studio Code, [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) inicia la aplicación y el servidor y activa el depurador CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="ea607-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="ea607-176">En Visual Studio para Mac, [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) inicia la aplicación y el servidor.</span><span class="sxs-lookup"><span data-stu-id="ea607-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="ea607-177">Al iniciar una aplicación desde un símbolo del sistema en la carpeta del proyecto, [dotnet run](/dotnet/core/tools/dotnet-run) inicia la aplicación y el servidor (solo Kestrel y HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="ea607-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="ea607-178">La configuración se especifica mediante la opción `-c|--configuration`, que está establecida en `Debug` (valor predeterminado) o `Release`.</span><span class="sxs-lookup"><span data-stu-id="ea607-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="ea607-179">Si no hay perfiles de inicio en un archivo *launchSettings.json*, use la opción `--launch-profile <NAME>` para establecer el perfil de inicio (por ejemplo, `Development` o `Production`).</span><span class="sxs-lookup"><span data-stu-id="ea607-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="ea607-180">Para más información, vea los temas [dotnet run](/dotnet/core/tools/dotnet-run) y [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="ea607-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="ea607-181">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="ea607-181">HTTP/2 support</span></span>

<span data-ttu-id="ea607-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación siguientes:</span><span class="sxs-lookup"><span data-stu-id="ea607-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="ea607-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ea607-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="ea607-184">Sistema operativo</span><span class="sxs-lookup"><span data-stu-id="ea607-184">Operating system</span></span>
    * <span data-ttu-id="ea607-185">Windows Server 2012 R2 o Windows 8.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="ea607-186">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="ea607-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="ea607-187">HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="ea607-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="ea607-188">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="ea607-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ea607-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="ea607-190">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="ea607-191">Plataforma de destino: no aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ea607-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="ea607-192">IIS (en proceso)</span><span class="sxs-lookup"><span data-stu-id="ea607-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ea607-193">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ea607-194">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="ea607-195">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="ea607-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ea607-196">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ea607-197">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="ea607-197">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ea607-198">Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="ea607-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="ea607-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="ea607-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="ea607-200">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="ea607-201">Plataforma de destino: no aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="ea607-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="ea607-202">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="ea607-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="ea607-203">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="ea607-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="ea607-204">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="ea607-204">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="ea607-205">Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="ea607-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="ea607-206">Una conexión HTTP/2 debe usar [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) y TLS 1.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="ea607-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="ea607-207">Para más información, vea los temas que pertenecen a los escenarios de implementación de servidor.</span><span class="sxs-lookup"><span data-stu-id="ea607-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea607-208">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ea607-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="ea607-209"><xref:fundamentals/servers/httpsys> (para ASP.NET Core 1.x, vea <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="ea607-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
