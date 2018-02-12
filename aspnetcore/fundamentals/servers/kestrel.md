---
title: "Implementación del servidor web Kestrel en ASP.NET Core"
author: tdykstra
description: "Este artículo presenta Kestrel, el servidor web multiplataforma para ASP.NET Core basado en libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="12d9f-103">Introducción a la implementación del servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12d9f-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="12d9f-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="12d9f-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="12d9f-105">Kestrel es un [servidor web multiplataforma para ASP.NET Core](index.md) basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica y multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="12d9f-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="12d9f-106">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12d9f-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="12d9f-107">Kestrel admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="12d9f-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="12d9f-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="12d9f-108">HTTPS</span></span>
  * <span data-ttu-id="12d9f-109">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="12d9f-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="12d9f-110">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="12d9f-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="12d9f-111">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="12d9f-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-113">[Vea o descargue el código de ejemplo para 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12d9f-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="12d9f-115">[Vea o descargue el código de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12d9f-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="12d9f-116">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="12d9f-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-118">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="12d9f-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="12d9f-119">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="12d9f-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="12d9f-122">También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.</span><span class="sxs-lookup"><span data-stu-id="12d9f-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="12d9f-124">Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="12d9f-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="12d9f-126">Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="12d9f-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="12d9f-127">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="12d9f-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="12d9f-129">Se necesita un proxy inverso para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="12d9f-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="12d9f-130">Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques.</span><span class="sxs-lookup"><span data-stu-id="12d9f-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="12d9f-131">Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño y los límites de conexiones simultáneas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="12d9f-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="12d9f-132">Un escenario en el que se requiere un proxy inverso sería cuando varias aplicaciones comparten el mismo puerto y dirección IP, y se ejecutan en un solo servidor.</span><span class="sxs-lookup"><span data-stu-id="12d9f-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="12d9f-133">Esto no funciona con Kestrel directamente, porque Kestrel no admite compartir la misma dirección IP y puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="12d9f-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="12d9f-134">Al configurar Kestrel para escuchar en un puerto, se controla todo el tráfico de ese puerto, independientemente del encabezado de host.</span><span class="sxs-lookup"><span data-stu-id="12d9f-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="12d9f-135">Después, debe reenviar a Kestrel un proxy inverso que pueda compartir puertos en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="12d9f-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="12d9f-136">Aunque no sea necesario un servidor proxy inverso, puede ser útil usar uno por otras razones:</span><span class="sxs-lookup"><span data-stu-id="12d9f-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="12d9f-137">Puede limitar el área de superficie expuesta.</span><span class="sxs-lookup"><span data-stu-id="12d9f-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="12d9f-138">Proporciona una capa de configuración y defensa adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="12d9f-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="12d9f-139">Es posible que se integre mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="12d9f-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="12d9f-140">Simplifica el equilibrio de carga y la configuración SSL.</span><span class="sxs-lookup"><span data-stu-id="12d9f-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="12d9f-141">Solo el servidor proxy inverso requiere un certificado SSL y dicho servidor puede comunicarse con los servidores de aplicaciones en la red interna mediante HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="12d9f-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="12d9f-142">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12d9f-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-144">El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="12d9f-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="12d9f-145">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="12d9f-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="12d9f-146">En *Program.cs*, el código de plantilla llama a `CreateDefaultBuilder`, que a su vez llama a [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="12d9f-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="12d9f-147">Si tiene que configurar las opciones de Kestrel, llame a `UseKestrel` en *Program.cs*, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12d9f-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="12d9f-149">Instale el paquete NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="12d9f-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="12d9f-150">Llame al método de extensión [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en `WebHostBuilder` en el método `Main` y especifique cualquier [opción de Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que necesite, como se muestra en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="12d9f-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="12d9f-151">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="12d9f-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-153">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="12d9f-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="12d9f-154">Estos son algunos de los límites que puede establecer:</span><span class="sxs-lookup"><span data-stu-id="12d9f-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="12d9f-155">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="12d9f-155">Maximum client connections</span></span>
- <span data-ttu-id="12d9f-156">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="12d9f-156">Maximum request body size</span></span>
- <span data-ttu-id="12d9f-157">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="12d9f-157">Minimum request body data rate</span></span>

<span data-ttu-id="12d9f-158">Establezca estas restricciones y otras en la propiedad `Limits` de la clase [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="12d9f-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="12d9f-159">La propiedad `Limits` contiene una instancia de la clase [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs).</span><span class="sxs-lookup"><span data-stu-id="12d9f-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="12d9f-160">**Conexiones máximas de cliente**</span><span class="sxs-lookup"><span data-stu-id="12d9f-160">**Maximum client connections**</span></span>

<span data-ttu-id="12d9f-161">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="12d9f-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="12d9f-162">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="12d9f-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="12d9f-163">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="12d9f-164">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="12d9f-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="12d9f-165">**Tamaño máximo del cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="12d9f-165">**Maximum request body size**</span></span>

<span data-ttu-id="12d9f-166">El tamaño máximo predeterminado del cuerpo de la solicitud es 30 000 000 bytes, que es aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="12d9f-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="12d9f-167">La manera recomendada de invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="12d9f-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="12d9f-168">Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="12d9f-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="12d9f-169">Puede invalidar la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="12d9f-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="12d9f-170">Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="12d9f-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="12d9f-171">Hay una propiedad `IsReadOnly` que indica si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="12d9f-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="12d9f-172">**Velocidad mínima de los datos del cuerpo de la solicitud**</span><span class="sxs-lookup"><span data-stu-id="12d9f-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="12d9f-173">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="12d9f-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="12d9f-174">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="12d9f-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="12d9f-175">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="12d9f-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="12d9f-176">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="12d9f-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="12d9f-177">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="12d9f-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="12d9f-178">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="12d9f-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="12d9f-179">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="12d9f-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="12d9f-180">Puede configurar las velocidades por solicitud de middleware:</span><span class="sxs-lookup"><span data-stu-id="12d9f-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="12d9f-181">Para saber más sobre otras opciones de Kestrel, vea estas clases:</span><span class="sxs-lookup"><span data-stu-id="12d9f-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="12d9f-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="12d9f-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="12d9f-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="12d9f-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="12d9f-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="12d9f-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="12d9f-186">Para más información sobre las opciones de Kestrel, vea [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) (Clase KestrelServerOptions).</span><span class="sxs-lookup"><span data-stu-id="12d9f-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="12d9f-187">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="12d9f-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-189">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="12d9f-190">Configure los prefijos de URL y los puertos en los que escuchará Kestrel mediante una llamada a los métodos `Listen` o `ListenUnixSocket` en `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="12d9f-191">(`UseUrls`, el argumento de línea de comandos `urls` y la variable de entorno ASPNETCORE_URLS también funcionan pero tienen las limitaciones que se indican [más adelante en este artículo](#useurls-limitations)).</span><span class="sxs-lookup"><span data-stu-id="12d9f-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="12d9f-192">**Enlazar a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="12d9f-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="12d9f-193">El método `Listen` se enlaza a un socket TCP y una lambda de opciones permite configurar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="12d9f-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="12d9f-194">Observe cómo este ejemplo configura SSL para un punto de conexión determinado mediante el uso de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="12d9f-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="12d9f-195">Puede usar la misma API para configurar otras opciones de Kestrel para puntos de conexión determinados.</span><span class="sxs-lookup"><span data-stu-id="12d9f-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="12d9f-196">**Enlazar a un socket de Unix**</span><span class="sxs-lookup"><span data-stu-id="12d9f-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="12d9f-197">Puede escuchar en un socket de Unix para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="12d9f-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="12d9f-198">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="12d9f-198">**Port 0**</span></span>

<span data-ttu-id="12d9f-199">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="12d9f-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="12d9f-200">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="12d9f-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="12d9f-201">**Limitaciones de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="12d9f-201">**UseUrls limitations**</span></span>

<span data-ttu-id="12d9f-202">Puede configurar puntos de conexión mediante una llamada al método `UseUrls` o usando el argumento de línea de comandos `urls` o la variable de entorno ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="12d9f-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="12d9f-203">Estos métodos son útiles si quiere que el código funcione con servidores distintos de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="12d9f-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="12d9f-204">Pero tenga en cuenta estas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="12d9f-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="12d9f-205">No se puede usar SSL con estos métodos.</span><span class="sxs-lookup"><span data-stu-id="12d9f-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="12d9f-206">Si usa los métodos `Listen` y `UseUrls`, los puntos de conexión `Listen` invalidan los puntos de conexión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="12d9f-207">**Configuración de puntos de conexión para IIS**</span><span class="sxs-lookup"><span data-stu-id="12d9f-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="12d9f-208">Si usa IIS, los enlaces de URL para IIS invalidan todos los enlaces que se establecen mediante una llamada a `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="12d9f-209">Para más información, vea [Introduction to ASP.NET Core Module](aspnet-core-module.md) (Introducción al módulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="12d9f-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="12d9f-211">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="12d9f-212">Puede configurar los prefijos de URL y los puertos en los que escuchará Kestrel mediante el método de extensión `UseUrls`, el argumento de línea de comandos `urls` o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12d9f-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="12d9f-213">Para más información sobre estos métodos, vea [Hosting](../../fundamentals/hosting.md) (Hospedaje en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="12d9f-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="12d9f-214">Para más información sobre el funcionamiento de enlace de URL cuando se usa IIS como proxy inverso, vea [ASP.NET Core Module](aspnet-core-module.md) (Introducción al módulo ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="12d9f-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="12d9f-215">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="12d9f-215">URL prefixes</span></span>

<span data-ttu-id="12d9f-216">Si se llama a `UseUrls` o se usa el argumento de línea de comandos `urls` o una variable de entorno ASPNETCORE_URLS, los prefijos de URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="12d9f-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="12d9f-218">Solo son válidos los prefijos de URL HTTP; Kestrel no admite SSL al configurar enlaces de URL mediante el uso de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="12d9f-219">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="12d9f-220">0.0.0.0 es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="12d9f-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="12d9f-221">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="12d9f-222">[::] es el equivalente de IPv4 0.0.0.0 para IPv6.</span><span class="sxs-lookup"><span data-stu-id="12d9f-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="12d9f-223">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="12d9f-224">Los nombres de host \* y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="12d9f-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="12d9f-225">Todo lo que no sea una dirección IP o un "localhost" reconocido se enlazará a todas las direcciones IP de IPv6 y IPv4.</span><span class="sxs-lookup"><span data-stu-id="12d9f-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="12d9f-226">Si tiene que enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](httpsys.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="12d9f-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="12d9f-227">Nombre de "localhost" con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="12d9f-228">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="12d9f-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="12d9f-229">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="12d9f-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="12d9f-230">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="12d9f-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="12d9f-232">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="12d9f-233">0.0.0.0 es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="12d9f-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="12d9f-234">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="12d9f-235">[::] es el equivalente de IPv4 0.0.0.0 para IPv6.</span><span class="sxs-lookup"><span data-stu-id="12d9f-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="12d9f-236">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="12d9f-237">Los nombres de host \* y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="12d9f-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="12d9f-238">Todo lo que no sea una dirección IP o un "localhost" reconocido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="12d9f-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="12d9f-239">Si tiene que enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](weblistener.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="12d9f-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="12d9f-240">Nombre de "localhost" con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="12d9f-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="12d9f-241">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="12d9f-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="12d9f-242">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="12d9f-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="12d9f-243">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="12d9f-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="12d9f-244">Socket de UNIX</span><span class="sxs-lookup"><span data-stu-id="12d9f-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="12d9f-245">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="12d9f-245">**Port 0**</span></span>

<span data-ttu-id="12d9f-246">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="12d9f-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="12d9f-247">El enlace al puerto 0 está permitido para cualquier nombre de host o IP excepto para el nombre `localhost`.</span><span class="sxs-lookup"><span data-stu-id="12d9f-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="12d9f-248">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="12d9f-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="12d9f-249">**Prefijos de URL para SSL**</span><span class="sxs-lookup"><span data-stu-id="12d9f-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="12d9f-250">No olvide incluir los prefijos de URL con `https:` si se llama al método de extensión `UseHttps`, tal y como se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="12d9f-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="12d9f-251">HTTP y HTTPS no se pueden hospedar en el mismo puerto.</span><span class="sxs-lookup"><span data-stu-id="12d9f-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="12d9f-252">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="12d9f-252">Next steps</span></span>

<span data-ttu-id="12d9f-253">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="12d9f-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="12d9f-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="12d9f-255">Aplicación de ejemplo para 2.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="12d9f-256">Código fuente de Kestrel</span><span class="sxs-lookup"><span data-stu-id="12d9f-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="12d9f-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="12d9f-258">Aplicación de ejemplo para 1.x</span><span class="sxs-lookup"><span data-stu-id="12d9f-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="12d9f-259">Código fuente de Kestrel</span><span class="sxs-lookup"><span data-stu-id="12d9f-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
