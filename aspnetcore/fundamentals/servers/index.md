---
title: Implementaciones de servidores web en ASP.NET Core
author: guardrex
description: Detecte los servidores web Kestrel y HTTP.sys de ASP.NET Core. Obtenga más información sobre cómo elegir un servidor y cuándo se debe usar un servidor proxy inverso.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861361"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="0355f-104">Implementaciones de servidores web en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0355f-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="0355f-105">Por [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) y [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="0355f-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="0355f-106">Una aplicación ASP.NET Core se ejecuta con una implementación de servidor HTTP en proceso.</span><span class="sxs-lookup"><span data-stu-id="0355f-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="0355f-107">La implementación del servidor realiza escuchas de solicitudes HTTP y las muestra en la aplicación como conjuntos de [características de solicitud](xref:fundamentals/request-features) compuestos en un <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="0355f-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="0355f-108">Windows</span><span class="sxs-lookup"><span data-stu-id="0355f-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="0355f-109">ASP.NET Core se suministra con los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="0355f-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="0355f-110">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="0355f-111">El servidor HTTP de IIS (`IISHttpServer`) es una implementación del [servidor en proceso de IIS](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) que se usa con el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0355f-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="0355f-112">El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="0355f-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="0355f-113">(HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).</span><span class="sxs-lookup"><span data-stu-id="0355f-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0355f-114">macOS</span><span class="sxs-lookup"><span data-stu-id="0355f-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="0355f-115">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0355f-116">Linux</span><span class="sxs-lookup"><span data-stu-id="0355f-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="0355f-117">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="0355f-118">Windows</span><span class="sxs-lookup"><span data-stu-id="0355f-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="0355f-119">ASP.NET Core se suministra con los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="0355f-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="0355f-120">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="0355f-121">El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor HTTP solo de Windows basado en el [controlador del kernel HTTP.sys y la API de servidor HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="0355f-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="0355f-122">(HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x).</span><span class="sxs-lookup"><span data-stu-id="0355f-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0355f-123">macOS</span><span class="sxs-lookup"><span data-stu-id="0355f-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="0355f-124">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0355f-125">Linux</span><span class="sxs-lookup"><span data-stu-id="0355f-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="0355f-126">ASP.NET Core se distribuye con el [servidor Kestrel](xref:fundamentals/servers/kestrel), que es el servidor HTTP multiplataforma predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0355f-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="0355f-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="0355f-127">Kestrel</span></span>

<span data-ttu-id="0355f-128">Kestrel es el servidor web predeterminado que se incluye en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0355f-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0355f-129">Kestrel se puede usar:</span><span class="sxs-lookup"><span data-stu-id="0355f-129">Kestrel can be used:</span></span>

* <span data-ttu-id="0355f-130">Por sí solo como un servidor perimetral que procesa solicitudes directamente desde una red, incluida Internet.</span><span class="sxs-lookup"><span data-stu-id="0355f-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="0355f-131">Con un *servidor proxy inverso*, tal como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0355f-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="0355f-132">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0355f-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="0355f-135">Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0355f-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="0355f-136">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="0355f-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0355f-137">Si la aplicación únicamente acepta solicitudes de una red interna, Kestrel puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="0355f-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="0355f-139">Si la aplicación se expone a Internet, Kestrel debe usar un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache ](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="0355f-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="0355f-140">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0355f-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="0355f-142">El motivo más importante por el que se debe usar un proxy inverso para las implementaciones de servidores perimetrales de acceso público expuestos directamente a Internet es la seguridad.</span><span class="sxs-lookup"><span data-stu-id="0355f-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="0355f-143">Las versiones 1.x de Kestrel no incluyen características de seguridad importantes para defenderse contra ataques procedentes de Internet.</span><span class="sxs-lookup"><span data-stu-id="0355f-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="0355f-144">Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño de solicitud y los límites de conexiones simultáneas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="0355f-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="0355f-145">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="0355f-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="0355f-146">IIS con Kestrel</span><span class="sxs-lookup"><span data-stu-id="0355f-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0355f-147">Cuando se usa [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), la aplicación de ASP.NET Core se ejecuta en el mismo proceso que el proceso de trabajo de IIS (el modelo de hospedaje *en proceso*) o en un proceso independiente del proceso de trabajo de IIS (el modelo de hospedaje *fuera de proceso*).</span><span class="sxs-lookup"><span data-stu-id="0355f-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="0355f-148">El [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) es un módulo nativo de IIS que controla las solicitudes de IIS nativas entre el servidor HTTP de IIS en proceso o el servidor Kestrel fuera de proceso.</span><span class="sxs-lookup"><span data-stu-id="0355f-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="0355f-149">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0355f-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0355f-150">Al usar [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) o [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) como proxy inverso para ASP.NET Core, la aplicación ASP.NET Core se ejecuta en un proceso independiente del proceso de trabajo de IIS.</span><span class="sxs-lookup"><span data-stu-id="0355f-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="0355f-151">En el proceso IIS, el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) coordina la relación de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="0355f-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="0355f-152">Las funciones principales del módulo ASP.NET Core consisten en iniciar la aplicación, reiniciarla cuando se bloquea y reenviar el tráfico HTTP a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0355f-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="0355f-153">Para obtener más información, vea <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0355f-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="0355f-154">Nginx con Kestrel</span><span class="sxs-lookup"><span data-stu-id="0355f-154">Nginx with Kestrel</span></span>

<span data-ttu-id="0355f-155">Para información sobre cómo usar Nginx en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="0355f-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="0355f-156">Apache con Kestrel</span><span class="sxs-lookup"><span data-stu-id="0355f-156">Apache with Kestrel</span></span>

<span data-ttu-id="0355f-157">Para información sobre cómo usar Apache en Linux como servidor proxy inverso para Kestrel, consulte <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="0355f-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="0355f-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="0355f-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0355f-159">Si las aplicaciones ASP.NET Core se ejecutan en Windows, HTTP.sys es una alternativa a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0355f-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="0355f-160">Suele recomendarse Kestrel para un rendimiento óptimo.</span><span class="sxs-lookup"><span data-stu-id="0355f-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="0355f-161">HTTP.sys se puede usar en escenarios en los que la aplicación se expone a Internet y las funcionalidades necesarias son compatibles con HTTP.sys pero no con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0355f-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="0355f-162">Para obtener más información, vea <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="0355f-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="0355f-164">HTTP.sys también se puede usar para las aplicaciones que solo se exponen a una red interna.</span><span class="sxs-lookup"><span data-stu-id="0355f-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0355f-166">HTTP.sys se denomina [WebListener](xref:fundamentals/servers/weblistener) en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0355f-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="0355f-167">Si las aplicaciones ASP.NET Core se ejecutan en Windows, WebListener es una alternativa para los escenarios en los que IIS no está disponible para hospedar aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="0355f-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![WebListener se comunica directamente con Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="0355f-169">WebListener también se puede usar en lugar de Kestrel para las aplicaciones que solo se exponen a una red interna, en el caso de que las funcionalidades necesarias sean compatibles con WebListener pero no con Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0355f-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="0355f-170">Para información sobre WebListener, vea [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="0355f-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![WebListener se comunica directamente con la red interna](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="0355f-172">Infraestructura de servidores de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0355f-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="0355f-173">La interfaz [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible en el método `Startup.Configure` expone la propiedad [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) de tipo [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="0355f-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="0355f-174">Kestrel y HTTP.sys (WebListener en ASP.NET Core 1.x) solo exponen una característica cada uno, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), pero otras implementaciones de servidor pueden exponer funcionalidades adicionales.</span><span class="sxs-lookup"><span data-stu-id="0355f-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="0355f-175">Se puede usar `IServerAddressesFeature` para averiguar a qué puerto se ha enlazado la implementación del servidor en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0355f-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="0355f-176">Servidores personalizados</span><span class="sxs-lookup"><span data-stu-id="0355f-176">Custom servers</span></span>

<span data-ttu-id="0355f-177">Si los servidores integrados no cumplen los requisitos de la aplicación, se puede crear una implementación de servidor personalizado.</span><span class="sxs-lookup"><span data-stu-id="0355f-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="0355f-178">En la [guía de Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) se muestra cómo escribir una implementación de [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) basada en [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="0355f-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="0355f-179">Solo requieren la implementación las interfaces de características que usa la aplicación, aunque como mínimo se debe admitir [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) e [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="0355f-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="0355f-180">Inicio del servidor</span><span class="sxs-lookup"><span data-stu-id="0355f-180">Server startup</span></span>

<span data-ttu-id="0355f-181">Cuando se usa [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio para Mac](https://www.visualstudio.com/vs/mac/) o [Visual Studio Code](https://code.visualstudio.com/), el servidor se inicia cuando el entorno de desarrollo integrado (IDE) inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0355f-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="0355f-182">En Visual Studio en Windows, se pueden usar perfiles de inicio para iniciar la aplicación y el servidor con [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) o la consola.</span><span class="sxs-lookup"><span data-stu-id="0355f-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="0355f-183">En Visual Studio Code, [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode) inicia la aplicación y el servidor y activa el depurador CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="0355f-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="0355f-184">En Visual Studio para Mac, [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) inicia la aplicación y el servidor.</span><span class="sxs-lookup"><span data-stu-id="0355f-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="0355f-185">Al iniciar una aplicación desde un símbolo del sistema en la carpeta del proyecto, [dotnet run](/dotnet/core/tools/dotnet-run) inicia la aplicación y el servidor (solo Kestrel y HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="0355f-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="0355f-186">La configuración se especifica mediante la opción `-c|--configuration`, que está establecida en `Debug` (valor predeterminado) o `Release`.</span><span class="sxs-lookup"><span data-stu-id="0355f-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="0355f-187">Si no hay perfiles de inicio en un archivo *launchSettings.json*, use la opción `--launch-profile <NAME>` para establecer el perfil de inicio (por ejemplo, `Development` o `Production`).</span><span class="sxs-lookup"><span data-stu-id="0355f-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="0355f-188">Para más información, vea los temas [dotnet run](/dotnet/core/tools/dotnet-run) y [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="0355f-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="0355f-189">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="0355f-189">HTTP/2 support</span></span>

<span data-ttu-id="0355f-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación siguientes:</span><span class="sxs-lookup"><span data-stu-id="0355f-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="0355f-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="0355f-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="0355f-192">Sistema operativo</span><span class="sxs-lookup"><span data-stu-id="0355f-192">Operating system</span></span>
    * <span data-ttu-id="0355f-193">Windows Server 2016/Windows 10 o posterior&dagger;</span><span class="sxs-lookup"><span data-stu-id="0355f-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="0355f-194">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="0355f-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="0355f-195">HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="0355f-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="0355f-196">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="0355f-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="0355f-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="0355f-198">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="0355f-199">Plataforma de destino: no aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="0355f-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="0355f-200">IIS (en proceso)</span><span class="sxs-lookup"><span data-stu-id="0355f-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="0355f-201">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0355f-202">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="0355f-203">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="0355f-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="0355f-204">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0355f-205">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="0355f-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="0355f-206">Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="0355f-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="0355f-207">&dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="0355f-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="0355f-208">La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada.</span><span class="sxs-lookup"><span data-stu-id="0355f-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="0355f-209">Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.</span><span class="sxs-lookup"><span data-stu-id="0355f-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="0355f-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="0355f-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="0355f-211">Windows Server 2016/Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="0355f-212">Plataforma de destino: no aplicable a implementaciones de HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="0355f-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="0355f-213">IIS (fuera de proceso)</span><span class="sxs-lookup"><span data-stu-id="0355f-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="0355f-214">Windows Server 2016/Windows 10 o posterior; IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="0355f-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="0355f-215">Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso a Kestrel usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="0355f-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="0355f-216">Plataforma de destino: no aplicable a implementaciones fuera de proceso de IIS.</span><span class="sxs-lookup"><span data-stu-id="0355f-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="0355f-217">Una conexión HTTP/2 debe usar [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) y TLS 1.2 o posterior.</span><span class="sxs-lookup"><span data-stu-id="0355f-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="0355f-218">Para más información, vea los temas que pertenecen a los escenarios de implementación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0355f-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0355f-219">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0355f-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="0355f-220"><xref:fundamentals/servers/httpsys> (para ASP.NET Core 1.x, vea <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="0355f-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
