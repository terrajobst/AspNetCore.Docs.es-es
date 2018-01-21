---
title: "Implementación de servidor web kestrel en ASP.NET Core"
author: tdykstra
description: "Presenta Kestrel, el servidor web de multiplataforma para ASP.NET Core según libuv."
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3695a6a127f77bd90538d72af6112ccf507f3482
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="7bf59-103">Introducción a la implementación de servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf59-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="7bf59-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), y [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="7bf59-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="7bf59-105">Kestrel es una plataforma de entre [servidor web de ASP.NET Core](index.md) basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica de multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="7bf59-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="7bf59-106">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bf59-106">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="7bf59-107">Kestrel admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7bf59-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="7bf59-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7bf59-108">HTTPS</span></span>
  * <span data-ttu-id="7bf59-109">Utilizada para habilitar la actualización opaca [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="7bf59-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="7bf59-110">Sockets de UNIX de alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="7bf59-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="7bf59-111">Kestrel es compatible con todas las plataformas y versiones que admite .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bf59-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-113">[Ver o descargar el código de ejemplo para 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bf59-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bf59-115">[Ver o descargar el código de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bf59-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="7bf59-116">Cuándo utilizar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="7bf59-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-118">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="7bf59-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="7bf59-119">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="7bf59-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="7bf59-122">También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.</span><span class="sxs-lookup"><span data-stu-id="7bf59-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bf59-124">Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="7bf59-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="7bf59-126">Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="7bf59-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="7bf59-127">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="7bf59-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="7bf59-129">Un proxy inverso es necesario para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="7bf59-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="7bf59-130">Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques.</span><span class="sxs-lookup"><span data-stu-id="7bf59-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="7bf59-131">Esto incluye, pero no se limita a los tiempos de espera adecuado, límites de tamaño y los límites de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="7bf59-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="7bf59-132">Un escenario que requiere a un proxy inverso es cuando tiene varias aplicaciones que comparten la misma dirección IP y puerto que se ejecuta en un solo servidor.</span><span class="sxs-lookup"><span data-stu-id="7bf59-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="7bf59-133">Esto no funciona con Kestrel directamente, porque Kestrel no es compatible con la misma dirección IP y puerto entre varios procesos de uso compartido.</span><span class="sxs-lookup"><span data-stu-id="7bf59-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="7bf59-134">Al configurar Kestrel para escuchar en un puerto, controla todo el tráfico de ese puerto independientemente del encabezado de host.</span><span class="sxs-lookup"><span data-stu-id="7bf59-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="7bf59-135">A continuación, debe reenviar un proxy inverso que puedan compartir puertos Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="7bf59-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="7bf59-136">Incluso si un servidor proxy inverso no es necesario, mediante uno podría ser una buena elección por otras razones:</span><span class="sxs-lookup"><span data-stu-id="7bf59-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="7bf59-137">Puede limitar la superficie expuesta.</span><span class="sxs-lookup"><span data-stu-id="7bf59-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="7bf59-138">Proporciona un nivel de configuración y una defensa adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="7bf59-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="7bf59-139">Es posible que integren mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="7bf59-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="7bf59-140">Esta característica simplifica el equilibrio de carga y la configuración SSL.</span><span class="sxs-lookup"><span data-stu-id="7bf59-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="7bf59-141">Solo el servidor proxy inverso requiere un certificado SSL y que el servidor puede comunicarse con los servidores de aplicaciones en la red interna mediante HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="7bf59-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="7bf59-142">Cómo usar Kestrel en aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bf59-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-144">El [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete se incluye en el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="7bf59-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="7bf59-145">Plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7bf59-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="7bf59-146">En *Program.cs*, las llamadas de código de plantilla `CreateDefaultBuilder`, que llama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7bf59-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="7bf59-147">Si tiene que configurar las opciones de Kestrel, llame a `UseKestrel` en *Program.cs* tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7bf59-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bf59-149">Instalar el [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7bf59-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="7bf59-150">Llame a la [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensión `WebHostBuilder` en su `Main` método especifica cualquier [opciones Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que necesite, como se muestra en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="7bf59-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="7bf59-151">Opciones de kestrel</span><span class="sxs-lookup"><span data-stu-id="7bf59-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-153">El servidor de web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="7bf59-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="7bf59-154">Estos son algunos de los límites que puede establecer:</span><span class="sxs-lookup"><span data-stu-id="7bf59-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="7bf59-155">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="7bf59-155">Maximum client connections</span></span>
- <span data-ttu-id="7bf59-156">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="7bf59-156">Maximum request body size</span></span>
- <span data-ttu-id="7bf59-157">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7bf59-157">Minimum request body data rate</span></span>

<span data-ttu-id="7bf59-158">Establezca estas restricciones y otras personas en la `Limits` propiedad de la [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) clase.</span><span class="sxs-lookup"><span data-stu-id="7bf59-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="7bf59-159">El `Limits` propiedad contiene una instancia de la [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) clase.</span><span class="sxs-lookup"><span data-stu-id="7bf59-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="7bf59-160">**Conexiones de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="7bf59-160">**Maximum client connections**</span></span>

<span data-ttu-id="7bf59-161">El número máximo de conexiones simultáneas de TCP abiertos se puede establecer para toda la aplicación con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7bf59-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="7bf59-162">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="7bf59-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="7bf59-163">Cuando se actualiza una conexión, no cuentan con la `MaxConcurrentConnections` límite.</span><span class="sxs-lookup"><span data-stu-id="7bf59-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="7bf59-164">El número máximo de conexiones es un número ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="7bf59-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="7bf59-165">**Tamaño del cuerpo de solicitud máximo**</span><span class="sxs-lookup"><span data-stu-id="7bf59-165">**Maximum request body size**</span></span>

<span data-ttu-id="7bf59-166">El tamaño predeterminado del cuerpo de solicitud máximo es: 30.000.000 bytes, que es aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="7bf59-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="7bf59-167">La manera recomendada para invalidar el límite de una aplicación de MVC de ASP.NET Core es usar el [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="7bf59-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="7bf59-168">Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación, todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="7bf59-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="7bf59-169">Puede invalidar la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="7bf59-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="7bf59-170">Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación se ha empezado la lectura de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7bf59-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="7bf59-171">Hay un `IsReadOnly` propiedad que indica si la `MaxRequestBodySize` propiedad está en estado de solo lectura, lo que significa es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="7bf59-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="7bf59-172">**Velocidad de datos de cuerpo de solicitud mínima**</span><span class="sxs-lookup"><span data-stu-id="7bf59-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="7bf59-173">Kestrel comprueba cada segundo si datos próximas novedades en a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="7bf59-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="7bf59-174">Si la velocidad se sitúa por debajo del mínimo, la conexión se agotó el tiempo. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la tasa durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="7bf59-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="7bf59-175">El período de gracia le ayuda a evitar las conexiones que inicialmente están enviando datos a una velocidad lento debido a TCP inicio lento.</span><span class="sxs-lookup"><span data-stu-id="7bf59-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="7bf59-176">El porcentaje mínimo predeterminado es 240 bytes por segundo, con un período de gracia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="7bf59-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="7bf59-177">Una velocidad mínima también se aplica a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7bf59-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="7bf59-178">El código para establecer el límite de solicitudes y el límite de respuesta es el mismo salvo tener `RequestBody` o `Response` en los nombres de propiedad y la interfaz.</span><span class="sxs-lookup"><span data-stu-id="7bf59-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="7bf59-179">Este es un ejemplo que muestra cómo configurar los tipos de datos mínimo en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bf59-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="7bf59-180">Puede configurar las tarifas por solicitud de middleware:</span><span class="sxs-lookup"><span data-stu-id="7bf59-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="7bf59-181">Para obtener información sobre otras opciones de Kestrel, vea las clases siguientes:</span><span class="sxs-lookup"><span data-stu-id="7bf59-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="7bf59-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="7bf59-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="7bf59-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="7bf59-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="7bf59-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="7bf59-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bf59-186">Para obtener información acerca de las opciones de Kestrel, consulte [KestrelServerOptions clase](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="7bf59-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="7bf59-187">Configuración de extremo</span><span class="sxs-lookup"><span data-stu-id="7bf59-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-189">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7bf59-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7bf59-190">Configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante una llamada a `Listen` o `ListenUnixSocket` métodos en `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="7bf59-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="7bf59-191">(`UseUrls`, `urls` argumento de línea de comandos y la variable de entorno ASPNETCORE_URLS también funcionan pero tienen las limitaciones se indicó [más adelante en este artículo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="7bf59-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="7bf59-192">**Enlazar a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="7bf59-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="7bf59-193">El `Listen` método se enlaza a un socket de TCP, y una lambda de opciones le permite configurar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="7bf59-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="7bf59-194">Observe cómo este ejemplo configura SSL para un extremo determinado mediante el uso de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="7bf59-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="7bf59-195">Puede utilizar las mismas API para configurar otras opciones de Kestrel para extremos determinados.</span><span class="sxs-lookup"><span data-stu-id="7bf59-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="7bf59-196">**Enlazar a un socket de Unix**</span><span class="sxs-lookup"><span data-stu-id="7bf59-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="7bf59-197">Puede escuchar en un socket de Unix para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7bf59-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="7bf59-198">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="7bf59-198">**Port 0**</span></span>

<span data-ttu-id="7bf59-199">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="7bf59-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="7bf59-200">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="7bf59-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="7bf59-201">**Limitaciones de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="7bf59-201">**UseUrls limitations**</span></span>

<span data-ttu-id="7bf59-202">Puede configurar los puntos de conexión mediante una llamada a la `UseUrls` método o utilizando el `urls` argumento de línea de comandos o la variable de entorno ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="7bf59-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="7bf59-203">Estos métodos son útiles si desea que su código para trabajar con servidores distintos Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7bf59-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="7bf59-204">Sin embargo, tenga en cuenta estas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="7bf59-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="7bf59-205">No se puede utilizar SSL con estos métodos.</span><span class="sxs-lookup"><span data-stu-id="7bf59-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="7bf59-206">Si se usan los la `Listen` método y `UseUrls`, el `Listen` extremos invalidación el `UseUrls` puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="7bf59-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="7bf59-207">**Configuración de extremo para IIS**</span><span class="sxs-lookup"><span data-stu-id="7bf59-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="7bf59-208">Si usa IIS, los enlaces de dirección URL para IIS invalidan todos los enlaces que se establecen mediante una llamada a `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7bf59-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="7bf59-209">Para obtener más información, consulte [Introducción a ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="7bf59-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7bf59-211">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7bf59-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="7bf59-212">Puede configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante el uso de la `UseUrls` método de extensión, el `urls` argumento de línea de comandos o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bf59-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="7bf59-213">Para obtener más información acerca de estos métodos, consulte [hospedaje](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="7bf59-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="7bf59-214">Para obtener información acerca del funcionamiento de enlace de dirección URL cuando se usa IIS como un proxy inverso, consulte [módulo principal de ASP.NET](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="7bf59-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="7bf59-215">Prefijos de dirección URL</span><span class="sxs-lookup"><span data-stu-id="7bf59-215">URL prefixes</span></span>

<span data-ttu-id="7bf59-216">Si se llama a `UseUrls` o usar el `urls` argumento de línea de comandos o una variable de entorno de ASPNETCORE_URLS, los prefijos de dirección URL pueden ser de cualquiera de los formatos siguientes.</span><span class="sxs-lookup"><span data-stu-id="7bf59-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7bf59-218">Prefijos de dirección URL de HTTP solo son válidos; Kestrel no admite SSL al configurar los enlaces de dirección URL mediante `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="7bf59-218">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="7bf59-219">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="7bf59-220">0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="7bf59-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="7bf59-221">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="7bf59-222">[::] es el equivalente de IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="7bf59-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="7bf59-223">Nombre de host con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="7bf59-224">Hospedar nombres, \*, y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="7bf59-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="7bf59-225">Todo lo que no es una dirección IP o el "localhost" reconocido se enlazará a todas las direcciones IP de IPv6 y IPv4.</span><span class="sxs-lookup"><span data-stu-id="7bf59-225">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="7bf59-226">Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](httpsys.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="7bf59-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="7bf59-227">Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="7bf59-228">Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="7bf59-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="7bf59-229">Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="7bf59-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="7bf59-230">Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="7bf59-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="7bf59-232">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="7bf59-233">0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="7bf59-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="7bf59-234">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="7bf59-235">[::] es el equivalente de IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="7bf59-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="7bf59-236">Nombre de host con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="7bf59-237">Los nombres, de host \*, y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="7bf59-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="7bf59-238">Todo lo que no es una dirección IP o el "localhost" reconocido enlaza a todas las direcciones IP de IPv6 y IPv4.</span><span class="sxs-lookup"><span data-stu-id="7bf59-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="7bf59-239">Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](weblistener.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="7bf59-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="7bf59-240">Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="7bf59-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="7bf59-241">Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="7bf59-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="7bf59-242">Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="7bf59-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="7bf59-243">Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="7bf59-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="7bf59-244">Sockets de UNIX</span><span class="sxs-lookup"><span data-stu-id="7bf59-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="7bf59-245">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="7bf59-245">**Port 0**</span></span>

<span data-ttu-id="7bf59-246">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="7bf59-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="7bf59-247">Enlace de puerto 0 se permite para cualquier nombre de host o una dirección IP excepto `localhost` nombre.</span><span class="sxs-lookup"><span data-stu-id="7bf59-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="7bf59-248">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="7bf59-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="7bf59-249">**Prefijos de dirección URL de SSL**</span><span class="sxs-lookup"><span data-stu-id="7bf59-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="7bf59-250">No olvide incluir los prefijos de dirección URL con `https:` si se llama a la `UseHttps` método de extensión, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7bf59-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="7bf59-251">HTTP y HTTPS no se puede hospedar en el mismo puerto.</span><span class="sxs-lookup"><span data-stu-id="7bf59-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="7bf59-252">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7bf59-252">Next steps</span></span>

<span data-ttu-id="7bf59-253">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7bf59-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bf59-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="7bf59-255">Aplicación de ejemplo de 2.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="7bf59-256">Código fuente de kestrel</span><span class="sxs-lookup"><span data-stu-id="7bf59-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bf59-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="7bf59-258">Aplicación de ejemplo para 1.x</span><span class="sxs-lookup"><span data-stu-id="7bf59-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="7bf59-259">Código fuente de kestrel</span><span class="sxs-lookup"><span data-stu-id="7bf59-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
