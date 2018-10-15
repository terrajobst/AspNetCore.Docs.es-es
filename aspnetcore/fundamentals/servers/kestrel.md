---
title: Implementación del servidor web Kestrel en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre Kestrel, el servidor web multiplataforma de ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 218b6429462991659ba02804fdc8fbb99b69f1a6
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211096"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="6c66f-103">Implementación del servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c66f-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="6c66f-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="6c66f-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="6c66f-105">Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="6c66f-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="6c66f-106">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c66f-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="6c66f-107">Kestrel admite las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="6c66f-107">Kestrel supports the following features:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="6c66f-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6c66f-108">HTTPS</span></span>
* <span data-ttu-id="6c66f-109">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="6c66f-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6c66f-110">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="6c66f-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="6c66f-111">HTTP/2 (excepto en macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="6c66f-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="6c66f-112">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="6c66f-113">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6c66f-113">HTTPS</span></span>
* <span data-ttu-id="6c66f-114">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="6c66f-114">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="6c66f-115">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="6c66f-115">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="6c66f-116">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6c66f-116">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="6c66f-117">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c66f-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="6c66f-118">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="6c66f-118">HTTP/2 support</span></span>

<span data-ttu-id="6c66f-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) está disponible para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:</span><span class="sxs-lookup"><span data-stu-id="6c66f-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="6c66f-120">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="6c66f-120">Operating system&dagger;</span></span>
  * <span data-ttu-id="6c66f-121">Windows Server 2012 R2 o Windows 8.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="6c66f-121">Windows Server 2012 R2/Windows 8.1 or later</span></span>
  * <span data-ttu-id="6c66f-122">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="6c66f-122">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="6c66f-123">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="6c66f-123">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="6c66f-124">Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="6c66f-124">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="6c66f-125">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="6c66f-125">TLS 1.2 or later connection</span></span>

<span data-ttu-id="6c66f-126">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-126">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="6c66f-127">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="6c66f-128">HTTP/2 está deshabilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="6c66f-129">Para más información sobre la configuración, vea las secciones de [opciones de Kestrel](#kestrel-options) y [configuración del punto de conexión](#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="6c66f-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [Endpoint configuration](#endpoint-configuration) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="6c66f-130">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="6c66f-130">When to use Kestrel with a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-131">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="6c66f-131">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="6c66f-132">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6c66f-135">Cualquiera de las configuraciones&mdash;con o sin un servidor proxy inverso&mdash;es una configuración de hospedaje válida y admitida para ASP.NET 2.0 o aplicaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="6c66f-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c66f-136">Si una aplicación acepta únicamente solicitudes de una red interna, Kestrel se puede usar directamente como servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6c66f-136">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="6c66f-138">Si expone la aplicación en Internet, use IIS, Nginx o Apache como *servidor proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="6c66f-138">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="6c66f-139">Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-139">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="6c66f-141">Se necesita un proxy inverso para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="6c66f-141">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="6c66f-142">Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques, como unos tiempos de espera, unos límites de tamaño y unos límites de conexiones simultáneas adecuados.</span><span class="sxs-lookup"><span data-stu-id="6c66f-142">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="6c66f-143">Un escenario de proxy inverso tiene lugar cuando varias aplicaciones comparten el mismo puerto y dirección IP, y se ejecutan en un solo servidor.</span><span class="sxs-lookup"><span data-stu-id="6c66f-143">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="6c66f-144">Este escenario no es viable con Kestrel, ya que Kestrel no permite compartir la misma dirección IP y el mismo puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="6c66f-144">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="6c66f-145">Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente del encabezado de host de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6c66f-145">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="6c66f-146">Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="6c66f-146">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="6c66f-147">Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil:</span><span class="sxs-lookup"><span data-stu-id="6c66f-147">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="6c66f-148">Puede limitar el área expuesta públicamente de las aplicaciones que hospeda.</span><span class="sxs-lookup"><span data-stu-id="6c66f-148">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="6c66f-149">Proporciona una capa extra de configuración y defensa.</span><span class="sxs-lookup"><span data-stu-id="6c66f-149">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="6c66f-150">Es posible que se integre mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="6c66f-150">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="6c66f-151">Simplifica el equilibrio de carga y la configuración SSL.</span><span class="sxs-lookup"><span data-stu-id="6c66f-151">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="6c66f-152">Solo el servidor proxy inverso requiere un certificado SSL, y dicho servidor se puede comunicar con los servidores de aplicaciones en la red interna por medio de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="6c66f-152">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="6c66f-153">Si no usa un proxy inverso con el [filtrado de hosts](#host-filtering) habilitado, deberá habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-153">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="6c66f-154">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c66f-154">How to use Kestrel in ASP.NET Core apps</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-155">El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="6c66f-155">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="6c66f-156">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-156">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="6c66f-157">En *Program.cs*, el código de plantilla llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que a su vez llama a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="6c66f-157">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6c66f-158">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, use `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="6c66f-158">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

<span data-ttu-id="6c66f-159">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, llame a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="6c66f-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c66f-160">Instale el paquete NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="6c66f-160">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="6c66f-161">Llame al método de extensión [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) en el método `Main`, y especifique cualquier [opción de Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) que necesite, como se muestra en la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="6c66f-161">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="6c66f-162">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="6c66f-162">Kestrel options</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-163">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="6c66f-163">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="6c66f-164">Estos son algunos de los límites importantes que se pueden personalizar:</span><span class="sxs-lookup"><span data-stu-id="6c66f-164">A few important limits that can be customized:</span></span>

* <span data-ttu-id="6c66f-165">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="6c66f-165">Maximum client connections</span></span>
* <span data-ttu-id="6c66f-166">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="6c66f-166">Maximum request body size</span></span>
* <span data-ttu-id="6c66f-167">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="6c66f-167">Minimum request body data rate</span></span>

<span data-ttu-id="6c66f-168">Establezca estas y otras restricciones en la propiedad [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la clase [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="6c66f-168">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="6c66f-169">La propiedad `Limits` contiene una instancia de la clase [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="6c66f-169">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="6c66f-170">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="6c66f-170">Maximum client connections</span></span>

[<span data-ttu-id="6c66f-171">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="6c66f-171">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="6c66f-172">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="6c66f-172">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

<span data-ttu-id="6c66f-173">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="6c66f-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

<span data-ttu-id="6c66f-174">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6c66f-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="6c66f-175">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-176">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="6c66f-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="6c66f-177">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="6c66f-177">Maximum request body size</span></span>

[<span data-ttu-id="6c66f-178">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="6c66f-178">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="6c66f-179">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="6c66f-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="6c66f-180">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="6c66f-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

<span data-ttu-id="6c66f-181">Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="6c66f-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="6c66f-182">Puede invalidar la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="6c66f-182">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-183">Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6c66f-183">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="6c66f-184">Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="6c66f-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="6c66f-185">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="6c66f-185">Minimum request body data rate</span></span>

[<span data-ttu-id="6c66f-186">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="6c66f-186">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="6c66f-187">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="6c66f-187">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="6c66f-188">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-188">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="6c66f-189">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-189">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="6c66f-190">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="6c66f-190">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="6c66f-191">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="6c66f-191">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="6c66f-192">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6c66f-192">A minimum rate also applies to the response.</span></span> <span data-ttu-id="6c66f-193">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="6c66f-193">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="6c66f-194">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6c66f-194">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="6c66f-195">Puede configurar las velocidades por solicitud de middleware:</span><span class="sxs-lookup"><span data-stu-id="6c66f-195">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="6c66f-196">Secuencias máximas por conexión</span><span class="sxs-lookup"><span data-stu-id="6c66f-196">Maximum streams per connection</span></span>

<span data-ttu-id="6c66f-197">`Http2.MaxStreamsPerConnection` limita el número de secuencias de solicitudes simultáneas por conexión HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6c66f-197">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="6c66f-198">Se rechazarán las secuencias en exceso.</span><span class="sxs-lookup"><span data-stu-id="6c66f-198">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="6c66f-199">El valor predeterminado es 100.</span><span class="sxs-lookup"><span data-stu-id="6c66f-199">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="6c66f-200">Tamaño de la tabla de encabezado</span><span class="sxs-lookup"><span data-stu-id="6c66f-200">Header table size</span></span>

<span data-ttu-id="6c66f-201">El descodificador HPACK descomprime los encabezados HTTP para las conexiones HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6c66f-201">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="6c66f-202">`Http2.HeaderTableSize` limita el tamaño de la tabla de compresión de encabezado que usa el descodificador HPACK.</span><span class="sxs-lookup"><span data-stu-id="6c66f-202">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="6c66f-203">El valor se proporciona en octetos y debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="6c66f-203">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="6c66f-204">El valor predeterminado es 4096.</span><span class="sxs-lookup"><span data-stu-id="6c66f-204">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="6c66f-205">Tamaño máximo de marco</span><span class="sxs-lookup"><span data-stu-id="6c66f-205">Maximum frame size</span></span>

<span data-ttu-id="6c66f-206">`Http2.MaxFrameSize` indica el tamaño máximo de la carga útil de la trama de conexión HTTP/2 que se puede recibir.</span><span class="sxs-lookup"><span data-stu-id="6c66f-206">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="6c66f-207">El valor se proporciona en octetos y debe estar comprendido entre 2^14 (16 384) y 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="6c66f-207">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="6c66f-208">El valor predeterminado es 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="6c66f-208">The default value is 2^14 (16,384).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-209">Para más información sobre otras opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="6c66f-209">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="6c66f-210">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="6c66f-210">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="6c66f-211">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="6c66f-211">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="6c66f-212">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="6c66f-212">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c66f-213">Para más información sobre las opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="6c66f-213">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="6c66f-214">KestrelServerOptions (clase)</span><span class="sxs-lookup"><span data-stu-id="6c66f-214">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="6c66f-215">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="6c66f-215">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a><span data-ttu-id="6c66f-216">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="6c66f-216">Endpoint configuration</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c66f-217">ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-217">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6c66f-218">Llame a los métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) o [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) en [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar los puertos y los prefijos de dirección URL de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-218">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="6c66f-219">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección.</span><span class="sxs-lookup"><span data-stu-id="6c66f-219">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="6c66f-220">La clave de configuración de host `urls` debe proceder de la configuración del host, no de la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6c66f-220">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="6c66f-221">Si se agrega una clave y un valor `urls` a *appsettings.json*, ello no repercute en la configuración de host, porque el host se inicializa completamente cuando se lee la configuración del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="6c66f-221">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="6c66f-222">Con todo, para configurar el host se puede usar una clave `urls` de *appsettings.json* con [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) en el generador de hosts:</span><span class="sxs-lookup"><span data-stu-id="6c66f-222">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c66f-223">ASP.NET Core enlaza de forma predeterminada a:</span><span class="sxs-lookup"><span data-stu-id="6c66f-223">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="6c66f-224">`https://localhost:5001` (cuando hay presente un certificado de desarrollo local)</span><span class="sxs-lookup"><span data-stu-id="6c66f-224">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="6c66f-225">Un certificado de desarrollo se crea:</span><span class="sxs-lookup"><span data-stu-id="6c66f-225">A development certificate is created:</span></span>

* <span data-ttu-id="6c66f-226">Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-226">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="6c66f-227">Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="6c66f-227">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="6c66f-228">Algunos exploradores requieren que se conceda permiso explícito en el explorador para poder confiar en el certificado de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="6c66f-228">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="6c66f-229">ASP.NET Core 2.1 y las plantillas de proyecto posteriores configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6c66f-229">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="6c66f-230">Llame a los métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) o [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) en [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar los puertos y los prefijos de dirección URL de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-230">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="6c66f-231">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).</span><span class="sxs-lookup"><span data-stu-id="6c66f-231">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="6c66f-232">La configuración `KestrelServerOptions` de ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="6c66f-232">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="6c66f-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="6c66f-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="6c66f-234">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-234">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="6c66f-235">Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-235">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="6c66f-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="6c66f-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="6c66f-237">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-237">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="6c66f-238">Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-238">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="6c66f-239">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="6c66f-239">Configure(IConfiguration)</span></span>

<span data-ttu-id="6c66f-240">Crea un cargador de configuración para configurar Kestrel que toma una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-240">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="6c66f-241">El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-241">The configuration must be scoped to the configuration section for Kestrel.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="listenoptionsusehttps"></a><span data-ttu-id="6c66f-242">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="6c66f-242">ListenOptions.UseHttps</span></span>

<span data-ttu-id="6c66f-243">Configure Kestrel para que use HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-243">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="6c66f-244">Extensiones de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="6c66f-244">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="6c66f-245">`UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-245">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="6c66f-246">Produce una excepción si no hay ningún certificado predeterminado configurado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-246">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="6c66f-247">Parámetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="6c66f-247">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="6c66f-248">`filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6c66f-248">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="6c66f-249">`password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="6c66f-249">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="6c66f-250">`configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-250">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="6c66f-251">Devuelve `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-251">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="6c66f-252">`storeName` es el almacén de certificados desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-252">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="6c66f-253">`subject` es el nombre del sujeto del certificado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-253">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="6c66f-254">`allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="6c66f-254">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="6c66f-255">`location` es la ubicación del almacén desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-255">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="6c66f-256">`serverCertificate` es el certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="6c66f-256">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="6c66f-257">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="6c66f-257">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6c66f-258">Como mínimo, debe existir un certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-258">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="6c66f-259">Estas son las configuraciones compatibles:</span><span class="sxs-lookup"><span data-stu-id="6c66f-259">Supported configurations described next:</span></span>

* <span data-ttu-id="6c66f-260">Sin configuración</span><span class="sxs-lookup"><span data-stu-id="6c66f-260">No configuration</span></span>
* <span data-ttu-id="6c66f-261">Reemplazar el certificado predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="6c66f-261">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="6c66f-262">Cambiar los valores predeterminados en el código</span><span class="sxs-lookup"><span data-stu-id="6c66f-262">Change the defaults in code</span></span>

<span data-ttu-id="6c66f-263">*Sin configuración*</span><span class="sxs-lookup"><span data-stu-id="6c66f-263">*No configuration*</span></span>

<span data-ttu-id="6c66f-264">Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="6c66f-264">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="6c66f-265">Especifique direcciones URL mediante los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="6c66f-265">Specify URLs using the:</span></span>

* <span data-ttu-id="6c66f-266">La variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-266">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="6c66f-267">El argumento de la línea de comandos `--urls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-267">`--urls` command-line argument.</span></span>
* <span data-ttu-id="6c66f-268">La clave de configuración de host `urls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-268">`urls` host configuration key.</span></span>
* <span data-ttu-id="6c66f-269">El método de extensión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-269">`UseUrls` extension method.</span></span>

<span data-ttu-id="6c66f-270">Para obtener más información, vea [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="6c66f-270">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="6c66f-271">El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="6c66f-271">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="6c66f-272">Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="6c66f-272">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="6c66f-273">*Reemplazar el certificado predeterminado de configuración*</span><span class="sxs-lookup"><span data-stu-id="6c66f-273">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="6c66f-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="6c66f-275">Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-275">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="6c66f-276">Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="6c66f-276">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="6c66f-277">En el siguiente ejemplo de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6c66f-277">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="6c66f-278">Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).</span><span class="sxs-lookup"><span data-stu-id="6c66f-278">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="6c66f-279">Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-279">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="6c66f-280">Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="6c66f-280">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="6c66f-281">Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:</span><span class="sxs-lookup"><span data-stu-id="6c66f-281">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="6c66f-282">Notas sobre el esquema:</span><span class="sxs-lookup"><span data-stu-id="6c66f-282">Schema notes:</span></span>

* <span data-ttu-id="6c66f-283">En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6c66f-283">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="6c66f-284">Por ejemplo, `HTTPS` y `Https` son válidos.</span><span class="sxs-lookup"><span data-stu-id="6c66f-284">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="6c66f-285">El parámetro `Url` es necesario en cada punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="6c66f-285">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="6c66f-286">El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.</span><span class="sxs-lookup"><span data-stu-id="6c66f-286">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="6c66f-287">En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="6c66f-287">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="6c66f-288">Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="6c66f-288">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="6c66f-289">La sección `Certificate` es opcional.</span><span class="sxs-lookup"><span data-stu-id="6c66f-289">The `Certificate` section is optional.</span></span> <span data-ttu-id="6c66f-290">Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores.</span><span class="sxs-lookup"><span data-stu-id="6c66f-290">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="6c66f-291">Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.</span><span class="sxs-lookup"><span data-stu-id="6c66f-291">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="6c66f-292">La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="6c66f-292">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="6c66f-293">Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.</span><span class="sxs-lookup"><span data-stu-id="6c66f-293">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="6c66f-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, options => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:</span><span class="sxs-lookup"><span data-stu-id="6c66f-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="6c66f-295">También puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para proseguir con la iteración en el cargador existente, como la proporcionada por [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="6c66f-295">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="6c66f-296">La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-296">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="6c66f-297">Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("Kestrel"))` con otra sección.</span><span class="sxs-lookup"><span data-stu-id="6c66f-297">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="6c66f-298">Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores.</span><span class="sxs-lookup"><span data-stu-id="6c66f-298">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="6c66f-299">El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-299">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="6c66f-300">`KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-300">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="6c66f-301">En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.</span><span class="sxs-lookup"><span data-stu-id="6c66f-301">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="6c66f-302">*Cambiar los valores predeterminados en el código*</span><span class="sxs-lookup"><span data-stu-id="6c66f-302">*Change the defaults in code*</span></span>

<span data-ttu-id="6c66f-303">`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior.</span><span class="sxs-lookup"><span data-stu-id="6c66f-303">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="6c66f-304">Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="6c66f-304">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="6c66f-305">*Compatibilidad de Kestrel con SNI*</span><span class="sxs-lookup"><span data-stu-id="6c66f-305">*Kestrel support for SNI*</span></span>

<span data-ttu-id="6c66f-306">[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="6c66f-306">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="6c66f-307">Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto.</span><span class="sxs-lookup"><span data-stu-id="6c66f-307">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="6c66f-308">El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-308">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="6c66f-309">Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-309">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="6c66f-310">La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="6c66f-310">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="6c66f-311">La compatibilidad con SNI requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6c66f-311">SNI support requires:</span></span>

* <span data-ttu-id="6c66f-312">Ejecutarse en el marco de destino `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-312">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="6c66f-313">En `netcoreapp2.0` y `net461`, se invoca la devolución de llamada, pero `name` siempre es `null`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-313">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="6c66f-314">`name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-314">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="6c66f-315">Todos los sitios web deben ejecutarse en la misma instancia de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-315">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="6c66f-316">Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="6c66f-316">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="6c66f-317">Enlazar a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="6c66f-317">Bind to a TCP socket</span></span>

<span data-ttu-id="6c66f-318">El método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se enlaza a un socket TCP y una lambda de opciones hace posible la configuración de un certificado SSL:</span><span class="sxs-lookup"><span data-stu-id="6c66f-318">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Loopback, 8000);
            options.Listen(IPAddress.Loopback, 8001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-319">En el ejemplo se configura SSL para un punto de conexión con [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="6c66f-319">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="6c66f-320">Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.</span><span class="sxs-lookup"><span data-stu-id="6c66f-320">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="6c66f-321">Enlazar a un socket de Unix</span><span class="sxs-lookup"><span data-stu-id="6c66f-321">Bind to a Unix socket</span></span>

<span data-ttu-id="6c66f-322">Escuche en un socket de Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para mejorar el rendimiento con Nginx, tal y como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6c66f-322">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a><span data-ttu-id="6c66f-323">Puerto 0</span><span class="sxs-lookup"><span data-stu-id="6c66f-323">Port 0</span></span>

<span data-ttu-id="6c66f-324">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="6c66f-324">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6c66f-325">En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="6c66f-325">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="6c66f-326">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6c66f-326">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="6c66f-327">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="6c66f-327">Limitations</span></span>

<span data-ttu-id="6c66f-328">Configure puntos de conexión con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="6c66f-328">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="6c66f-329">UseUrls</span><span class="sxs-lookup"><span data-stu-id="6c66f-329">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="6c66f-330">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="6c66f-330">`--urls` command-line argument</span></span>
* <span data-ttu-id="6c66f-331">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="6c66f-331">`urls` host configuration key</span></span>
* <span data-ttu-id="6c66f-332">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6c66f-332">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6c66f-333">Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-333">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="6c66f-334">Sin embargo, tenga en cuenta las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="6c66f-334">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="6c66f-335">SSL no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).</span><span class="sxs-lookup"><span data-stu-id="6c66f-335">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="6c66f-336">Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-336">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="6c66f-337">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="6c66f-337">IIS endpoint configuration</span></span>

<span data-ttu-id="6c66f-338">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-338">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="6c66f-339">Para más información, vea el tema [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6c66f-339">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c66f-340">ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-340">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="6c66f-341">Configure los puertos y los prefijos de dirección URL para Kernel usando lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6c66f-341">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="6c66f-342">El método de extensión [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="6c66f-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="6c66f-343">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="6c66f-343">`--urls` command-line argument</span></span>
* <span data-ttu-id="6c66f-344">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="6c66f-344">`urls` host configuration key</span></span>
* <span data-ttu-id="6c66f-345">El sistema de configuración de ASP.NET Core, incluida la variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6c66f-345">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="6c66f-346">Para más información sobre estos métodos, vea [Hospedaje](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="6c66f-346">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="6c66f-347">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="6c66f-347">IIS endpoint configuration</span></span>

<span data-ttu-id="6c66f-348">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-348">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="6c66f-349">Para más información, vea el tema [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6c66f-349">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="6c66f-350">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="6c66f-350">ListenOptions.Protocols</span></span>

<span data-ttu-id="6c66f-351">La propiedad `Protocols` establece los protocolos HTTP (`HttpProtocols`) habilitados en un punto de conexión o para el servidor.</span><span class="sxs-lookup"><span data-stu-id="6c66f-351">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="6c66f-352">Asigne un valor a la propiedad `Protocols` desde el valor de enumeración `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-352">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="6c66f-353">Valor de enumeración `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="6c66f-353">`HttpProtocols` enum value</span></span> | <span data-ttu-id="6c66f-354">Protocolo de conexión permitido</span><span class="sxs-lookup"><span data-stu-id="6c66f-354">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="6c66f-355">HTTP/1.1 solo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-355">HTTP/1.1 only.</span></span> <span data-ttu-id="6c66f-356">Puede usarse con o sin TLS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-356">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="6c66f-357">HTTP/2 solo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-357">HTTP/2 only.</span></span> <span data-ttu-id="6c66f-358">Se usa principalmente con TLS.</span><span class="sxs-lookup"><span data-stu-id="6c66f-358">Primarily used with TLS.</span></span> <span data-ttu-id="6c66f-359">Se pueden utilizar sin TLS solo si el cliente admite un [modo de conocimientos previos](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="6c66f-359">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="6c66f-360">HTTP/1.1 y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="6c66f-360">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="6c66f-361">Requiere una conexión TLS y [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) para negociar HTTP/2; de lo contrario, la opción predeterminada para la conexión es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6c66f-361">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="6c66f-362">El protocolo predeterminado es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="6c66f-362">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="6c66f-363">Restricciones de TLS para HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="6c66f-363">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="6c66f-364">TLS 1.2 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="6c66f-364">TLS version 1.2 or later</span></span>
* <span data-ttu-id="6c66f-365">Renegociación deshabilitada</span><span class="sxs-lookup"><span data-stu-id="6c66f-365">Renegotiation disabled</span></span>
* <span data-ttu-id="6c66f-366">Compresión deshabilitada</span><span class="sxs-lookup"><span data-stu-id="6c66f-366">Compression disabled</span></span>
* <span data-ttu-id="6c66f-367">Tamaños de intercambio de claves efímeras mínimos:</span><span class="sxs-lookup"><span data-stu-id="6c66f-367">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="6c66f-368">Curva elíptica Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;; 224 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="6c66f-368">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="6c66f-369">Campo finito Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;; 2048 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="6c66f-369">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="6c66f-370">Conjunto de cifrado no restringido</span><span class="sxs-lookup"><span data-stu-id="6c66f-370">Cipher suite not blacklisted</span></span>

<span data-ttu-id="6c66f-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con la curva elíptica P-256 &lbrack;`FIPS186`&rbrack; es compatible de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="6c66f-372">El siguiente ejemplo permite conexiones HTTP/1.1 y HTTP/2 en el puerto 8000.</span><span class="sxs-lookup"><span data-stu-id="6c66f-372">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="6c66f-373">Las conexiones se protegen mediante TLS con un certificado proporcionado:</span><span class="sxs-lookup"><span data-stu-id="6c66f-373">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

<span data-ttu-id="6c66f-374">Opcionalmente, cree una implementación `IConnectionAdapter` para filtrar los protocolos de enlace TLS en una base por conexión para los cifrados específicos:</span><span class="sxs-lookup"><span data-stu-id="6c66f-374">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="6c66f-375">*Defina el protocolo a partir de la configuración*</span><span class="sxs-lookup"><span data-stu-id="6c66f-375">*Set the protocol from configuration*</span></span>

<span data-ttu-id="6c66f-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6c66f-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="6c66f-377">En el siguiente ejemplo de *appsettings.json*, se establece un protocolo de conexión predeterminado (HTTP/1.1 y HTTP/2) para todos los puntos de conexión de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="6c66f-377">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="6c66f-378">El siguiente ejemplo de archivo de configuración establece un protocolo de conexión para un punto de conexión específico:</span><span class="sxs-lookup"><span data-stu-id="6c66f-378">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="6c66f-379">Los protocolos especificados en el código invalidan los valores establecidos por la configuración.</span><span class="sxs-lookup"><span data-stu-id="6c66f-379">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="6c66f-380">Configuración de transporte</span><span class="sxs-lookup"><span data-stu-id="6c66f-380">Transport configuration</span></span>

<span data-ttu-id="6c66f-381">Desde el lanzamiento de ASP.NET Core 2.1, el transporte predeterminado de Kestrel deja de basarse en Libuv y pasa a basarse en sockets administrados.</span><span class="sxs-lookup"><span data-stu-id="6c66f-381">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="6c66f-382">Se trata de un cambio muy importante para las aplicaciones ASP.NET Core 2.0 que actualizan a 2.1 y llaman a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv), y que dependen de cualquiera de los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="6c66f-382">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="6c66f-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referencia de paquete directa)</span><span class="sxs-lookup"><span data-stu-id="6c66f-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="6c66f-384">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="6c66f-384">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="6c66f-385">En el caso de los proyectos de ASP.NET Core 2.1 o posterior que usan el metapaquete [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) y requieren el uso de Libuv:</span><span class="sxs-lookup"><span data-stu-id="6c66f-385">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="6c66f-386">Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6c66f-386">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="6c66f-387">Llame a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="6c66f-387">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="6c66f-388">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="6c66f-388">URL prefixes</span></span>

<span data-ttu-id="6c66f-389">Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="6c66f-389">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-390">Solo son válidos los prefijos de dirección URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c66f-390">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="6c66f-391">Kestrel no admite SSL cuando se configuran enlaces de dirección URL con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-391">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="6c66f-392">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-392">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="6c66f-393">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-393">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6c66f-394">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-394">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="6c66f-395">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-395">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6c66f-396">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-396">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="6c66f-397">Los nombres de host, `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="6c66f-397">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="6c66f-398">Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-398">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6c66f-399">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="6c66f-399">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="6c66f-400">Si no usa un proxy inverso con el [filtrado de hosts](#host-filtering) habilitado, habilítelo.</span><span class="sxs-lookup"><span data-stu-id="6c66f-400">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="6c66f-401">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-401">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6c66f-402">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="6c66f-402">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6c66f-403">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-403">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6c66f-404">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="6c66f-404">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="6c66f-405">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-405">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="6c66f-406">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-406">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="6c66f-407">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-407">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="6c66f-408">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-408">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="6c66f-409">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-409">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="6c66f-410">Los nombres de host `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="6c66f-410">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="6c66f-411">Todo lo que no sea una dirección IP o un `localhost` reconocido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="6c66f-411">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="6c66f-412">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](xref:fundamentals/servers/weblistener) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="6c66f-412">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="6c66f-413">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="6c66f-413">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="6c66f-414">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="6c66f-414">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="6c66f-415">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="6c66f-415">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="6c66f-416">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="6c66f-416">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="6c66f-417">Socket de UNIX</span><span class="sxs-lookup"><span data-stu-id="6c66f-417">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="6c66f-418">**Puerto 0**</span><span class="sxs-lookup"><span data-stu-id="6c66f-418">**Port 0**</span></span>

<span data-ttu-id="6c66f-419">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="6c66f-419">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="6c66f-420">El enlace al puerto `0` está permitido en cualquier nombre de host o IP, excepto en `localhost`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-420">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="6c66f-421">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6c66f-421">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="6c66f-422">**Prefijos de URL para SSL**</span><span class="sxs-lookup"><span data-stu-id="6c66f-422">**URL prefixes for SSL**</span></span>

<span data-ttu-id="6c66f-423">Si llama al método de extensión `UseHttps`, procure incluir los prefijos de URL con `https:`:</span><span class="sxs-lookup"><span data-stu-id="6c66f-423">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="6c66f-424">HTTP y HTTPS no se pueden hospedar en el mismo puerto.</span><span class="sxs-lookup"><span data-stu-id="6c66f-424">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a><span data-ttu-id="6c66f-425">Filtrado de hosts</span><span class="sxs-lookup"><span data-stu-id="6c66f-425">Host filtering</span></span>

<span data-ttu-id="6c66f-426">Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host.</span><span class="sxs-lookup"><span data-stu-id="6c66f-426">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="6c66f-427">El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido.</span><span class="sxs-lookup"><span data-stu-id="6c66f-427">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="6c66f-428">Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas.</span><span class="sxs-lookup"><span data-stu-id="6c66f-428">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="6c66f-429">Ninguno de estos datos se usa para validar encabezados `Host` de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="6c66f-429">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6c66f-430">Como solución alternativa, hospede detrás de un proxy inverso con filtrado de encabezados de host.</span><span class="sxs-lookup"><span data-stu-id="6c66f-430">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="6c66f-431">Este es el único escenario admitido de Kestrel en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6c66f-431">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c66f-432">Como solución alternativa, use un middleware para filtrar las solicitudes por el encabezado `Host`:</span><span class="sxs-lookup"><span data-stu-id="6c66f-432">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="6c66f-433">Registre el `HostFilteringMiddleware` anterior en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="6c66f-433">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="6c66f-434">Cabe mencionar que el [orden de registro del middleware](xref:fundamentals/middleware/index#order) es importante.</span><span class="sxs-lookup"><span data-stu-id="6c66f-434">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="6c66f-435">debe tener lugar inmediatamente después del registro del middleware de diagnóstico (por ejemplo, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="6c66f-435">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="6c66f-436">El middleware espera una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="6c66f-436">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6c66f-437">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="6c66f-437">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c66f-438">Como solución alternativa, use el Middleware de filtrado de hosts.</span><span class="sxs-lookup"><span data-stu-id="6c66f-438">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="6c66f-439">El Middleware de filtrado de hosts se facilita a través del paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="6c66f-439">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="6c66f-440">Este middleware se agrega por medio de [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que llama a [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="6c66f-440">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="6c66f-441">El Middleware de filtrado de hosts está deshabilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="6c66f-441">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="6c66f-442">Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="6c66f-442">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="6c66f-443">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="6c66f-443">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6c66f-444">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="6c66f-444">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="6c66f-445">El [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="6c66f-445">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="6c66f-446">El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios.</span><span class="sxs-lookup"><span data-stu-id="6c66f-446">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="6c66f-447">Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado de host no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga.</span><span class="sxs-lookup"><span data-stu-id="6c66f-447">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="6c66f-448">Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral, o cuando el encabezado de host se reenvía directamente.</span><span class="sxs-lookup"><span data-stu-id="6c66f-448">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="6c66f-449">Para más información sobre el Middleware de encabezados reenviados, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="6c66f-449">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6c66f-450">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6c66f-450">Additional resources</span></span>

* [<span data-ttu-id="6c66f-451">Aplicación de HTTPS</span><span class="sxs-lookup"><span data-stu-id="6c66f-451">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="6c66f-452">Código fuente de Kestrel</span><span class="sxs-lookup"><span data-stu-id="6c66f-452">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* <span data-ttu-id="6c66f-453">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Enrutamiento y sintaxis de mensajes [Sección 5.4: Host])</span><span class="sxs-lookup"><span data-stu-id="6c66f-453">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
* [<span data-ttu-id="6c66f-454">Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga</span><span class="sxs-lookup"><span data-stu-id="6c66f-454">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
