---
title: "Implementación de servidor web kestrel en ASP.NET Core"
author: tdykstra
description: "Presenta Kestrel, el servidor web de multiplataforma para ASP.NET Core según libuv."
keywords: "Núcleo de ASP.NET, Kestrel, libuv, prefijos de dirección url"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a961d46d7804b7ac7e570692fe42727feae3d5c9
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="5c7b1-104">Introducción a la implementación de servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c7b1-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="5c7b1-105">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), y [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="5c7b1-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="5c7b1-106">Kestrel es una plataforma de entre [servidor web de ASP.NET Core](index.md) basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica de multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="5c7b1-107">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="5c7b1-108">Kestrel admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="5c7b1-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="5c7b1-109">HTTPS</span></span>
  * <span data-ttu-id="5c7b1-110">Utilizada para habilitar la actualización opaca [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="5c7b1-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="5c7b1-111">Sockets de UNIX de alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="5c7b1-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="5c7b1-112">Kestrel es compatible con todas las plataformas y versiones que admite .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="5c7b1-114">Ver o descargar el código de ejemplo para 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-114">View or download sample code for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="5c7b1-116">Ver o descargar el código de ejemplo para 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-116">View or download sample code for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="5c7b1-117">Cuándo utilizar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="5c7b1-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c7b1-119">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="5c7b1-120">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="5c7b1-123">También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c7b1-125">Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="5c7b1-127">Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="5c7b1-128">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="5c7b1-130">Un proxy inverso es necesario para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="5c7b1-131">Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="5c7b1-132">Esto incluye, pero no se limita a los tiempos de espera adecuado, límites de tamaño y los límites de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="5c7b1-133">Un escenario que requiere a un proxy inverso es cuando tiene varias aplicaciones que comparten la misma dirección IP y puerto que se ejecuta en un solo servidor.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="5c7b1-134">Esto no funciona con Kestrel directamente, porque Kestrel no es compatible con la misma dirección IP y puerto entre varios procesos de uso compartido.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="5c7b1-135">Al configurar Kestrel para escuchar en un puerto, controla todo el tráfico de ese puerto independientemente del encabezado de host.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="5c7b1-136">A continuación, debe reenviar un proxy inverso que puedan compartir puertos Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="5c7b1-137">Incluso si un servidor proxy inverso no es necesario, mediante uno podría ser una buena elección por otras razones:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="5c7b1-138">Puede limitar la superficie expuesta.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="5c7b1-139">Proporciona un nivel de configuración y una defensa adicional opcional.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="5c7b1-140">Es posible que integren mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="5c7b1-141">Esta característica simplifica el equilibrio de carga y la configuración SSL.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="5c7b1-142">Solo el servidor proxy inverso requiere un certificado SSL y que el servidor puede comunicarse con los servidores de aplicaciones en la red interna mediante HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="5c7b1-143">Cómo usar Kestrel en aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c7b1-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c7b1-145">El [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete se incluye en el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="5c7b1-146">Plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="5c7b1-147">En *Program.cs*, las llamadas de código de plantilla `CreateDefaultBuilder`, que llama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="5c7b1-148">Si tiene que configurar las opciones de Kestrel, llame a `UseKestrel` en *Program.cs* tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c7b1-150">Instalar el [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="5c7b1-151">Llame a la [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensión `WebHostBuilder` en su `Main` método especifica cualquier [opciones Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que necesite, como se muestra en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="5c7b1-152">Opciones de kestrel</span><span class="sxs-lookup"><span data-stu-id="5c7b1-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c7b1-154">El servidor de web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="5c7b1-155">Estos son algunos de los límites que puede establecer:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="5c7b1-156">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="5c7b1-156">Maximum client connections</span></span>
- <span data-ttu-id="5c7b1-157">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="5c7b1-157">Maximum request body size</span></span>
- <span data-ttu-id="5c7b1-158">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-158">Minimum request body data rate</span></span>

<span data-ttu-id="5c7b1-159">Establezca estas restricciones y otras personas en la `Limits` propiedad de la [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) clase.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="5c7b1-160">El `Limits` propiedad contiene una instancia de la [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) clase.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="5c7b1-161">**Conexiones de cliente máximo**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-161">**Maximum client connections**</span></span>

<span data-ttu-id="5c7b1-162">El número máximo de conexiones simultáneas de TCP abiertos se puede establecer para toda la aplicación con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="5c7b1-163">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="5c7b1-164">Cuando se actualiza una conexión, no cuentan con la `MaxConcurrentConnections` límite.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="5c7b1-165">El número máximo de conexiones es un número ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="5c7b1-166">**Tamaño del cuerpo de solicitud máximo**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-166">**Maximum request body size**</span></span>

<span data-ttu-id="5c7b1-167">El tamaño predeterminado del cuerpo de solicitud máximo es: 30.000.000 bytes, que es aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="5c7b1-168">La manera recomendada para invalidar el límite de una aplicación de MVC de ASP.NET Core es usar el [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="5c7b1-169">Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación, todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="5c7b1-170">Puede invalidar la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="5c7b1-171">Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación se ha empezado la lectura de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="5c7b1-172">Hay un `IsReadOnly` propiedad que indica si la `MaxRequestBodySize` propiedad está en estado de solo lectura, lo que significa es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="5c7b1-173">**Velocidad de datos de cuerpo de solicitud mínima**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="5c7b1-174">Kestrel comprueba cada segundo si datos próximas novedades en a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="5c7b1-175">Si la velocidad se sitúa por debajo del mínimo, la conexión se agotó el tiempo. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la tasa durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="5c7b1-176">El período de gracia le ayuda a evitar las conexiones que inicialmente están enviando datos a una velocidad lento debido a TCP inicio lento.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="5c7b1-177">El porcentaje mínimo predeterminado es 240 bytes por segundo, con un período de gracia de 5 segundos.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="5c7b1-178">Una velocidad mínima también se aplica a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="5c7b1-179">El código para establecer el límite de solicitudes y el límite de respuesta es el mismo salvo tener `RequestBody` o `Response` en los nombres de propiedad y la interfaz.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="5c7b1-180">Este es un ejemplo que muestra cómo configurar los tipos de datos mínimo en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="5c7b1-181">Puede configurar las tarifas por solicitud de middleware:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="5c7b1-182">Para obtener información sobre otras opciones de Kestrel, vea las clases siguientes:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="5c7b1-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="5c7b1-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="5c7b1-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="5c7b1-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="5c7b1-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="5c7b1-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c7b1-187">Para obtener información acerca de las opciones de Kestrel, consulte [KestrelServerOptions clase](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="5c7b1-188">Configuración de extremo</span><span class="sxs-lookup"><span data-stu-id="5c7b1-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c7b1-190">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="5c7b1-191">Configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante una llamada a `Listen` o `ListenUnixSocket` métodos en `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="5c7b1-192">(`UseUrls`, `urls` argumento de línea de comandos y la variable de entorno ASPNETCORE_URLS también funcionan pero tienen las limitaciones se indicó [más adelante en este artículo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="5c7b1-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="5c7b1-193">**Enlazar a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="5c7b1-194">El `Listen` método se enlaza a un socket de TCP, y una lambda de opciones le permite configurar un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="5c7b1-195">Observe cómo este ejemplo configura SSL para un extremo determinado mediante el uso de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="5c7b1-196">Puede utilizar las mismas API para configurar otras opciones de Kestrel para extremos determinados.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="5c7b1-197">**Enlazar a un socket de Unix**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="5c7b1-198">Puede escuchar en un socket de Unix para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="5c7b1-199">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-199">**Port 0**</span></span>

<span data-ttu-id="5c7b1-200">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="5c7b1-201">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="5c7b1-202">**Limitaciones de UseUrls**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-202">**UseUrls limitations**</span></span>

<span data-ttu-id="5c7b1-203">Puede configurar los puntos de conexión mediante una llamada a la `UseUrls` método o utilizando el `urls` argumento de línea de comandos o la variable de entorno ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="5c7b1-204">Estos métodos son útiles si desea que su código para trabajar con servidores distintos Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="5c7b1-205">Sin embargo, tenga en cuenta estas limitaciones:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="5c7b1-206">No se puede utilizar SSL con estos métodos.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="5c7b1-207">Si se usan los la `Listen` método y `UseUrls`, el `Listen` extremos invalidación el `UseUrls` puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="5c7b1-208">**Configuración de extremo para IIS**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="5c7b1-209">Si usa IIS, los enlaces de dirección URL para IIS invalidan todos los enlaces que se establecen mediante una llamada a `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="5c7b1-210">Para obtener más información, consulte [Introducción a ASP.NET Core módulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c7b1-212">De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="5c7b1-213">Puede configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante el uso de la `UseUrls` método de extensión, el `urls` argumento de línea de comandos o el sistema de configuración de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="5c7b1-214">Para obtener más información acerca de estos métodos, consulte [hospedaje](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="5c7b1-215">Para obtener información acerca del funcionamiento de enlace de dirección URL cuando se usa IIS como un proxy inverso, consulte [módulo principal de ASP.NET](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b1-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="5c7b1-216">Prefijos de dirección URL</span><span class="sxs-lookup"><span data-stu-id="5c7b1-216">URL prefixes</span></span>

<span data-ttu-id="5c7b1-217">Si se llama a `UseUrls` o usar el `urls` argumento de línea de comandos o una variable de entorno de ASPNETCORE_URLS, los prefijos de dirección URL pueden ser de cualquiera de los formatos siguientes.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c7b1-219">Prefijos de dirección URL de HTTP solo son válidos; Kestrel no admite SSL al configurar los enlaces de dirección URL mediante `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="5c7b1-220">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="5c7b1-221">0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="5c7b1-222">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="5c7b1-223">[::] es el equivalente de IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="5c7b1-224">Nombre de host con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="5c7b1-225">Hospedar nombres, *, y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="5c7b1-226">Todo lo que no es una dirección IP o el "localhost" reconocido se enlazará a todas las direcciones IP de IPv6 y IPv4.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="5c7b1-227">Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](httpsys.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="5c7b1-228">Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="5c7b1-229">Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="5c7b1-230">Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="5c7b1-231">Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="5c7b1-233">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="5c7b1-234">0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="5c7b1-235">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="5c7b1-236">[::] es el equivalente de IPv6 de IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="5c7b1-237">Nombre de host con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="5c7b1-238">Los nombres, de host \*, y + no son especiales.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="5c7b1-239">Todo lo que no es una dirección IP o el "localhost" reconocido enlaza a todas las direcciones IP de IPv6 y IPv4.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="5c7b1-240">Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](weblistener.md) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="5c7b1-241">Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="5c7b1-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="5c7b1-242">Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="5c7b1-243">Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="5c7b1-244">Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="5c7b1-245">Sockets de UNIX</span><span class="sxs-lookup"><span data-stu-id="5c7b1-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="5c7b1-246">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-246">**Port 0**</span></span>

<span data-ttu-id="5c7b1-247">Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="5c7b1-248">Enlace de puerto 0 se permite para cualquier nombre de host o una dirección IP excepto `localhost` nombre.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="5c7b1-249">En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="5c7b1-250">**Prefijos de dirección URL de SSL**</span><span class="sxs-lookup"><span data-stu-id="5c7b1-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="5c7b1-251">No olvide incluir los prefijos de dirección URL con `https:` si se llama a la `UseHttps` método de extensión, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="5c7b1-252">HTTP y HTTPS no se puede hospedar en el mismo puerto.</span><span class="sxs-lookup"><span data-stu-id="5c7b1-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="5c7b1-253">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5c7b1-253">Next steps</span></span>

<span data-ttu-id="5c7b1-254">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="5c7b1-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c7b1-255">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="5c7b1-256">Aplicación de ejemplo de 2.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="5c7b1-257">Código fuente de kestrel</span><span class="sxs-lookup"><span data-stu-id="5c7b1-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c7b1-258">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="5c7b1-259">Aplicación de ejemplo para 1.x</span><span class="sxs-lookup"><span data-stu-id="5c7b1-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="5c7b1-260">Código fuente de kestrel</span><span class="sxs-lookup"><span data-stu-id="5c7b1-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
