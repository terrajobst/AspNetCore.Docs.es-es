---
title: Implementación del servidor web Kestrel en ASP.NET Core
author: guardrex
description: Obtenga información sobre Kestrel, el servidor web multiplataforma de ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/11/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: a85403468d64b35ac5b6754139f78a12ad3fc386
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249534"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d645a-103">Implementación del servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d645a-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d645a-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="d645a-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="d645a-105">Para ver la versión 1.1 de este tema, descargue [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf) [Implementación del servidor web Kestrel en ASP.NET Core (versión 1.1, PDF)].</span><span class="sxs-lookup"><span data-stu-id="d645a-105">For the 1.1 version of this topic, download [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="d645a-106">Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d645a-106">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d645a-107">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d645a-107">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="d645a-108">Kestrel admite los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="d645a-108">Kestrel supports the following scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d645a-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d645a-109">HTTPS</span></span>
* <span data-ttu-id="d645a-110">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="d645a-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="d645a-111">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="d645a-111">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="d645a-112">HTTP/2 (excepto en macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="d645a-112">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="d645a-113">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="d645a-113">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d645a-114">HTTPS</span><span class="sxs-lookup"><span data-stu-id="d645a-114">HTTPS</span></span>
* <span data-ttu-id="d645a-115">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="d645a-115">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="d645a-116">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="d645a-116">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="d645a-117">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d645a-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="d645a-118">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d645a-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="d645a-119">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d645a-119">HTTP/2 support</span></span>

<span data-ttu-id="d645a-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) está disponible para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:</span><span class="sxs-lookup"><span data-stu-id="d645a-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="d645a-121">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="d645a-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="d645a-122">Windows Server 2016/Windows 10 o posterior&Dagger;</span><span class="sxs-lookup"><span data-stu-id="d645a-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="d645a-123">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="d645a-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="d645a-124">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="d645a-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="d645a-125">Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="d645a-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="d645a-126">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="d645a-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="d645a-127">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="d645a-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="d645a-128">&Dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="d645a-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="d645a-129">La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada.</span><span class="sxs-lookup"><span data-stu-id="d645a-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="d645a-130">Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.</span><span class="sxs-lookup"><span data-stu-id="d645a-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="d645a-131">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="d645a-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="d645a-132">HTTP/2 está deshabilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d645a-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="d645a-133">Para obtener más información sobre la configuración, consulte las secciones [Opciones de Kestrel](#kestrel-options) y [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="d645a-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="d645a-134">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="d645a-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="d645a-135">Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d645a-135">You can use Kestrel by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="d645a-136">Un servidor proxy inverso recibe las solicitudes HTTP de la red y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-136">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="d645a-137">Kestrel empleado como servidor web perimetral (accesible desde Internet):</span><span class="sxs-lookup"><span data-stu-id="d645a-137">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="d645a-139">Kestrel empleado en una configuración de proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="d645a-139">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="d645a-141">Cualquiera de las configuraciones, &mdash;con o sin un servidor proxy inverso&mdash;, es una configuración de hospedaje admitida para ASP.NET 2.1 o aplicaciones posteriores que reciben solicitudes de Internet.</span><span class="sxs-lookup"><span data-stu-id="d645a-141">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.1 or later apps that receive requests from the Internet.</span></span>

<span data-ttu-id="d645a-142">Kestrel, utilizado como un servidor perimetral sin un servidor proxy inverso, no permite compartir la misma dirección IP y el mismo puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="d645a-142">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="d645a-143">Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente de los encabezados `Host` de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d645a-143">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="d645a-144">Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="d645a-144">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="d645a-145">Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil.</span><span class="sxs-lookup"><span data-stu-id="d645a-145">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="d645a-146">Un proxy inverso puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d645a-146">A reverse proxy:</span></span>

* <span data-ttu-id="d645a-147">Limitar el área expuesta públicamente de las aplicaciones que hospeda.</span><span class="sxs-lookup"><span data-stu-id="d645a-147">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="d645a-148">Proporcionar una capa extra de configuración y defensa.</span><span class="sxs-lookup"><span data-stu-id="d645a-148">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="d645a-149">Posiblemente, integrarse mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="d645a-149">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="d645a-150">Simplificar el equilibrio de carga y la configuración de una comunicación segura (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d645a-150">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="d645a-151">Solo el servidor proxy inverso requiere un certificado X.509, y dicho servidor se puede comunicar con los servidores de aplicaciones en la red interna por medio de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="d645a-151">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="d645a-152">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="d645a-152">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="d645a-153">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d645a-153">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="d645a-154">El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="d645a-154">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="d645a-155">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d645a-155">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="d645a-156">En *Program.cs*, el código de plantilla llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que a su vez llama a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="d645a-156">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d645a-157">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, use `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="d645a-157">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

<span data-ttu-id="d645a-158">Si la aplicación no llama a `CreateDefaultBuilder` para configurar el host, llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **antes** de llamar a `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="d645a-158">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d645a-159">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, llame a [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="d645a-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="d645a-160">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="d645a-160">Kestrel options</span></span>

<span data-ttu-id="d645a-161">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="d645a-161">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="d645a-162">Establezca restricciones en la propiedad [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) de la clase [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="d645a-162">Set constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="d645a-163">La propiedad `Limits` contiene una instancia de la clase [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="d645a-163">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="d645a-164">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="d645a-164">Maximum client connections</span></span>

[<span data-ttu-id="d645a-165">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="d645a-165">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="d645a-166">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="d645a-166">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="d645a-167">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="d645a-167">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="d645a-168">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="d645a-168">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="d645a-169">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="d645a-169">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="d645a-170">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="d645a-170">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="d645a-171">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="d645a-171">Maximum request body size</span></span>

[<span data-ttu-id="d645a-172">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="d645a-172">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="d645a-173">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="d645a-173">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="d645a-174">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="d645a-174">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d645a-175">Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="d645a-175">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="d645a-176">Puede invalidar la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="d645a-176">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

<span data-ttu-id="d645a-177">Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d645a-177">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="d645a-178">Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="d645a-178">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="d645a-179">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="d645a-179">Minimum request body data rate</span></span>

[<span data-ttu-id="d645a-180">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="d645a-180">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="d645a-181">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="d645a-181">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="d645a-182">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="d645a-182">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="d645a-183">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="d645a-183">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="d645a-184">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="d645a-184">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="d645a-185">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="d645a-185">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="d645a-186">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d645a-186">A minimum rate also applies to the response.</span></span> <span data-ttu-id="d645a-187">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="d645a-187">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="d645a-188">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d645a-188">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

<span data-ttu-id="d645a-189">Puede invalidar los límites de velocidad mínima por solicitud en el middleware:</span><span class="sxs-lookup"><span data-stu-id="d645a-189">You can override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d645a-190">Ninguna de las características de velocidad a las que se hace referencia en el ejemplo anterior están presentes en `HttpContext.Features` para las solicitudes HTTP/2, porque no se permite modificar los límites de velocidad por solicitud en HTTP/2 debido a la compatibilidad del protocolo con la multiplexación de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d645a-190">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="d645a-191">Se siguen aplicando límites de velocidad en todo el servidor configurados con `KestrelServerOptions.Limits` a las conexiones HTTP/1.x y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d645a-191">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="d645a-192">Secuencias máximas por conexión</span><span class="sxs-lookup"><span data-stu-id="d645a-192">Maximum streams per connection</span></span>

<span data-ttu-id="d645a-193">`Http2.MaxStreamsPerConnection` limita el número de secuencias de solicitudes simultáneas por conexión HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d645a-193">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="d645a-194">Se rechazarán las secuencias en exceso.</span><span class="sxs-lookup"><span data-stu-id="d645a-194">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="d645a-195">El valor predeterminado es 100.</span><span class="sxs-lookup"><span data-stu-id="d645a-195">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="d645a-196">Tamaño de la tabla de encabezado</span><span class="sxs-lookup"><span data-stu-id="d645a-196">Header table size</span></span>

<span data-ttu-id="d645a-197">El descodificador HPACK descomprime los encabezados HTTP para las conexiones HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d645a-197">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="d645a-198">`Http2.HeaderTableSize` limita el tamaño de la tabla de compresión de encabezado que usa el descodificador HPACK.</span><span class="sxs-lookup"><span data-stu-id="d645a-198">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="d645a-199">El valor se proporciona en octetos y debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="d645a-199">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="d645a-200">El valor predeterminado es 4096.</span><span class="sxs-lookup"><span data-stu-id="d645a-200">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="d645a-201">Tamaño máximo de marco</span><span class="sxs-lookup"><span data-stu-id="d645a-201">Maximum frame size</span></span>

<span data-ttu-id="d645a-202">`Http2.MaxFrameSize` indica el tamaño máximo de la carga útil de la trama de conexión HTTP/2 que se puede recibir.</span><span class="sxs-lookup"><span data-stu-id="d645a-202">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="d645a-203">El valor se proporciona en octetos y debe estar comprendido entre 2^14 (16 384) y 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="d645a-203">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="d645a-204">El valor predeterminado es 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="d645a-204">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="d645a-205">Tamaño máximo del encabezado de solicitud</span><span class="sxs-lookup"><span data-stu-id="d645a-205">Maximum request header size</span></span>

<span data-ttu-id="d645a-206">`Http2.MaxRequestHeaderFieldSize` indica el tamaño máximo permitido (en octetos) de los valores de los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="d645a-206">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="d645a-207">Este límite se aplica al nombre y al valor conjuntamente en sus representaciones comprimidas y no comprimidas.</span><span class="sxs-lookup"><span data-stu-id="d645a-207">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="d645a-208">El valor debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="d645a-208">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="d645a-209">El valor predeterminado es 8192.</span><span class="sxs-lookup"><span data-stu-id="d645a-209">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="d645a-210">Tamaño inicial de la ventana de conexión</span><span class="sxs-lookup"><span data-stu-id="d645a-210">Initial connection window size</span></span>

<span data-ttu-id="d645a-211">`Http2.InitialConnectionWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez de forma agregada en todas las solicitudes (transmisiones) por conexión.</span><span class="sxs-lookup"><span data-stu-id="d645a-211">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="d645a-212">Las solicitudes también están limitadas por `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="d645a-212">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="d645a-213">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="d645a-213">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="d645a-214">El valor predeterminado es 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="d645a-214">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="d645a-215">Tamaño inicial de la ventana de transmisión</span><span class="sxs-lookup"><span data-stu-id="d645a-215">Initial stream window size</span></span>

<span data-ttu-id="d645a-216">`Http2.InitialStreamWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez por solicitud (transmisión).</span><span class="sxs-lookup"><span data-stu-id="d645a-216">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="d645a-217">Las solicitudes también están limitadas por `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="d645a-217">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="d645a-218">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="d645a-218">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="d645a-219">El valor predeterminado es 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="d645a-219">The default value is 96 KB (98,304).</span></span>

::: moniker-end

<span data-ttu-id="d645a-220">Para más información sobre otras opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="d645a-220">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="d645a-221">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="d645a-221">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="d645a-222">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="d645a-222">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="d645a-223">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="d645a-223">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

## <a name="endpoint-configuration"></a><span data-ttu-id="d645a-224">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="d645a-224">Endpoint configuration</span></span>

<span data-ttu-id="d645a-225">ASP.NET Core enlaza de forma predeterminada a:</span><span class="sxs-lookup"><span data-stu-id="d645a-225">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="d645a-226">`https://localhost:5001` (cuando hay presente un certificado de desarrollo local)</span><span class="sxs-lookup"><span data-stu-id="d645a-226">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="d645a-227">Un certificado de desarrollo se crea:</span><span class="sxs-lookup"><span data-stu-id="d645a-227">A development certificate is created:</span></span>

* <span data-ttu-id="d645a-228">Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="d645a-228">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="d645a-229">Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="d645a-229">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="d645a-230">Algunos exploradores requieren que se conceda permiso explícito en el explorador para poder confiar en el certificado de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="d645a-230">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="d645a-231">ASP.NET Core 2.1 y las plantillas de proyecto posteriores configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="d645a-231">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d645a-232">Llame a los métodos [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) o [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) en [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) para configurar los puertos y los prefijos de dirección URL de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-232">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="d645a-233">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d645a-233">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="d645a-234">La configuración `KestrelServerOptions` de ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="d645a-234">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="d645a-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="d645a-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="d645a-236">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado.</span><span class="sxs-lookup"><span data-stu-id="d645a-236">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="d645a-237">Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="d645a-237">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="d645a-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="d645a-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="d645a-239">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d645a-239">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="d645a-240">Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="d645a-240">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="d645a-241">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="d645a-241">Configure(IConfiguration)</span></span>

<span data-ttu-id="d645a-242">Crea un cargador de configuración para configurar Kestrel que toma una [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) como entrada.</span><span class="sxs-lookup"><span data-stu-id="d645a-242">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="d645a-243">El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-243">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="d645a-244">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="d645a-244">ListenOptions.UseHttps</span></span>

<span data-ttu-id="d645a-245">Configure Kestrel para que use HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d645a-245">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="d645a-246">Extensiones de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="d645a-246">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="d645a-247">`UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d645a-247">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="d645a-248">Produce una excepción si no hay ningún certificado predeterminado configurado.</span><span class="sxs-lookup"><span data-stu-id="d645a-248">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="d645a-249">Parámetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="d645a-249">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="d645a-250">`filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d645a-250">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="d645a-251">`password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="d645a-251">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="d645a-252">`configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="d645a-252">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="d645a-253">Devuelve `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="d645a-253">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="d645a-254">`storeName` es el almacén de certificados desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="d645a-254">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="d645a-255">`subject` es el nombre del sujeto del certificado.</span><span class="sxs-lookup"><span data-stu-id="d645a-255">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="d645a-256">`allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="d645a-256">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="d645a-257">`location` es la ubicación del almacén desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="d645a-257">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="d645a-258">`serverCertificate` es el certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="d645a-258">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="d645a-259">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d645a-259">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="d645a-260">Como mínimo, debe existir un certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d645a-260">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="d645a-261">Estas son las configuraciones compatibles:</span><span class="sxs-lookup"><span data-stu-id="d645a-261">Supported configurations described next:</span></span>

* <span data-ttu-id="d645a-262">Sin configuración</span><span class="sxs-lookup"><span data-stu-id="d645a-262">No configuration</span></span>
* <span data-ttu-id="d645a-263">Reemplazar el certificado predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="d645a-263">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="d645a-264">Cambiar los valores predeterminados en el código</span><span class="sxs-lookup"><span data-stu-id="d645a-264">Change the defaults in code</span></span>

<span data-ttu-id="d645a-265">*Sin configuración*</span><span class="sxs-lookup"><span data-stu-id="d645a-265">*No configuration*</span></span>

<span data-ttu-id="d645a-266">Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="d645a-266">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="d645a-267">Especifique direcciones URL mediante los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="d645a-267">Specify URLs using the:</span></span>

* <span data-ttu-id="d645a-268">La variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="d645a-268">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="d645a-269">El argumento de la línea de comandos `--urls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-269">`--urls` command-line argument.</span></span>
* <span data-ttu-id="d645a-270">La clave de configuración de host `urls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-270">`urls` host configuration key.</span></span>
* <span data-ttu-id="d645a-271">El método de extensión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-271">`UseUrls` extension method.</span></span>

<span data-ttu-id="d645a-272">Para obtener más información, vea [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="d645a-272">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="d645a-273">El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="d645a-273">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="d645a-274">Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="d645a-274">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="d645a-275">*Reemplazar el certificado predeterminado de configuración*</span><span class="sxs-lookup"><span data-stu-id="d645a-275">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="d645a-276">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-276">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="d645a-277">Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-277">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="d645a-278">Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="d645a-278">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="d645a-279">En el siguiente ejemplo de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d645a-279">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="d645a-280">Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).</span><span class="sxs-lookup"><span data-stu-id="d645a-280">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="d645a-281">Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="d645a-281">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="d645a-282">Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="d645a-282">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="d645a-283">Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:</span><span class="sxs-lookup"><span data-stu-id="d645a-283">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="d645a-284">Notas sobre el esquema:</span><span class="sxs-lookup"><span data-stu-id="d645a-284">Schema notes:</span></span>

* <span data-ttu-id="d645a-285">En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d645a-285">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="d645a-286">Por ejemplo, `HTTPS` y `Https` son válidos.</span><span class="sxs-lookup"><span data-stu-id="d645a-286">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="d645a-287">El parámetro `Url` es necesario en cada punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="d645a-287">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="d645a-288">El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.</span><span class="sxs-lookup"><span data-stu-id="d645a-288">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="d645a-289">En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="d645a-289">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="d645a-290">Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="d645a-290">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="d645a-291">La sección `Certificate` es opcional.</span><span class="sxs-lookup"><span data-stu-id="d645a-291">The `Certificate` section is optional.</span></span> <span data-ttu-id="d645a-292">Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores.</span><span class="sxs-lookup"><span data-stu-id="d645a-292">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="d645a-293">Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.</span><span class="sxs-lookup"><span data-stu-id="d645a-293">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="d645a-294">La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="d645a-294">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="d645a-295">Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.</span><span class="sxs-lookup"><span data-stu-id="d645a-295">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="d645a-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, options => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:</span><span class="sxs-lookup"><span data-stu-id="d645a-296">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="d645a-297">También puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para proseguir con la iteración en el cargador existente, como la proporcionada por [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="d645a-297">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="d645a-298">La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.</span><span class="sxs-lookup"><span data-stu-id="d645a-298">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="d645a-299">Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("Kestrel"))` con otra sección.</span><span class="sxs-lookup"><span data-stu-id="d645a-299">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="d645a-300">Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores.</span><span class="sxs-lookup"><span data-stu-id="d645a-300">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="d645a-301">El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="d645a-301">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="d645a-302">`KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar.</span><span class="sxs-lookup"><span data-stu-id="d645a-302">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="d645a-303">En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.</span><span class="sxs-lookup"><span data-stu-id="d645a-303">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="d645a-304">*Cambiar los valores predeterminados en el código*</span><span class="sxs-lookup"><span data-stu-id="d645a-304">*Change the defaults in code*</span></span>

<span data-ttu-id="d645a-305">`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior.</span><span class="sxs-lookup"><span data-stu-id="d645a-305">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="d645a-306">Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="d645a-306">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="d645a-307">*Compatibilidad de Kestrel con SNI*</span><span class="sxs-lookup"><span data-stu-id="d645a-307">*Kestrel support for SNI*</span></span>

<span data-ttu-id="d645a-308">[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="d645a-308">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="d645a-309">Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto.</span><span class="sxs-lookup"><span data-stu-id="d645a-309">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="d645a-310">El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="d645a-310">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="d645a-311">Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="d645a-311">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="d645a-312">La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="d645a-312">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="d645a-313">La compatibilidad con SNI requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d645a-313">SNI support requires:</span></span>

* <span data-ttu-id="d645a-314">Ejecutarse en el marco de destino `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="d645a-314">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="d645a-315">En `netcoreapp2.0` y `net461`, se invoca la devolución de llamada, pero `name` siempre es `null`.</span><span class="sxs-lookup"><span data-stu-id="d645a-315">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="d645a-316">`name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="d645a-316">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="d645a-317">Todos los sitios web deben ejecutarse en la misma instancia de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-317">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="d645a-318">Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="d645a-318">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="d645a-319">Enlazar a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="d645a-319">Bind to a TCP socket</span></span>

<span data-ttu-id="d645a-320">El método [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) se enlaza a un socket TCP, y una expresión lambda de opciones permite configurar un certificado X.509:</span><span class="sxs-lookup"><span data-stu-id="d645a-320">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

<span data-ttu-id="d645a-321">En el ejemplo se configura HTTPS para un punto de conexión con [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="d645a-321">The example configures HTTPS for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="d645a-322">Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.</span><span class="sxs-lookup"><span data-stu-id="d645a-322">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="d645a-323">Enlazar a un socket de Unix</span><span class="sxs-lookup"><span data-stu-id="d645a-323">Bind to a Unix socket</span></span>

<span data-ttu-id="d645a-324">Escuche en un socket de Unix con [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) para mejorar el rendimiento con Nginx, tal y como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d645a-324">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a><span data-ttu-id="d645a-325">Puerto 0</span><span class="sxs-lookup"><span data-stu-id="d645a-325">Port 0</span></span>

<span data-ttu-id="d645a-326">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="d645a-326">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="d645a-327">En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="d645a-327">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="d645a-328">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d645a-328">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="d645a-329">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="d645a-329">Limitations</span></span>

<span data-ttu-id="d645a-330">Configure puntos de conexión con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="d645a-330">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="d645a-331">UseUrls</span><span class="sxs-lookup"><span data-stu-id="d645a-331">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="d645a-332">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="d645a-332">`--urls` command-line argument</span></span>
* <span data-ttu-id="d645a-333">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="d645a-333">`urls` host configuration key</span></span>
* <span data-ttu-id="d645a-334">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d645a-334">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="d645a-335">Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-335">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="d645a-336">Sin embargo, tenga en cuenta las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="d645a-336">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="d645a-337">HTTPS no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).</span><span class="sxs-lookup"><span data-stu-id="d645a-337">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="d645a-338">Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-338">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="d645a-339">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="d645a-339">IIS endpoint configuration</span></span>

<span data-ttu-id="d645a-340">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-340">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="d645a-341">Para más información, vea el tema [Módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d645a-341">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="d645a-342">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="d645a-342">ListenOptions.Protocols</span></span>

<span data-ttu-id="d645a-343">La propiedad `Protocols` establece los protocolos HTTP (`HttpProtocols`) habilitados en un punto de conexión o para el servidor.</span><span class="sxs-lookup"><span data-stu-id="d645a-343">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="d645a-344">Asigne un valor a la propiedad `Protocols` desde el valor de enumeración `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="d645a-344">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="d645a-345">Valor de enumeración `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="d645a-345">`HttpProtocols` enum value</span></span> | <span data-ttu-id="d645a-346">Protocolo de conexión permitido</span><span class="sxs-lookup"><span data-stu-id="d645a-346">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="d645a-347">HTTP/1.1 solo.</span><span class="sxs-lookup"><span data-stu-id="d645a-347">HTTP/1.1 only.</span></span> <span data-ttu-id="d645a-348">Puede usarse con o sin TLS.</span><span class="sxs-lookup"><span data-stu-id="d645a-348">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="d645a-349">HTTP/2 solo.</span><span class="sxs-lookup"><span data-stu-id="d645a-349">HTTP/2 only.</span></span> <span data-ttu-id="d645a-350">Se usa principalmente con TLS.</span><span class="sxs-lookup"><span data-stu-id="d645a-350">Primarily used with TLS.</span></span> <span data-ttu-id="d645a-351">Se pueden utilizar sin TLS solo si el cliente admite un [modo de conocimientos previos](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="d645a-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="d645a-352">HTTP/1.1 y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d645a-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="d645a-353">Requiere una conexión TLS y [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) para negociar HTTP/2; de lo contrario, la opción predeterminada para la conexión es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="d645a-353">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="d645a-354">El protocolo predeterminado es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="d645a-354">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="d645a-355">Restricciones de TLS para HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="d645a-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="d645a-356">TLS 1.2 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d645a-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="d645a-357">Renegociación deshabilitada</span><span class="sxs-lookup"><span data-stu-id="d645a-357">Renegotiation disabled</span></span>
* <span data-ttu-id="d645a-358">Compresión deshabilitada</span><span class="sxs-lookup"><span data-stu-id="d645a-358">Compression disabled</span></span>
* <span data-ttu-id="d645a-359">Tamaños de intercambio de claves efímeras mínimos:</span><span class="sxs-lookup"><span data-stu-id="d645a-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="d645a-360">Curva elíptica Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;; 224 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="d645a-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="d645a-361">Campo finito Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;; 2048 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="d645a-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="d645a-362">Conjunto de cifrado no restringido</span><span class="sxs-lookup"><span data-stu-id="d645a-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="d645a-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con la curva elíptica P-256 &lbrack;`FIPS186`&rbrack; es compatible de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d645a-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="d645a-364">El siguiente ejemplo permite conexiones HTTP/1.1 y HTTP/2 en el puerto 8000.</span><span class="sxs-lookup"><span data-stu-id="d645a-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="d645a-365">Las conexiones se protegen mediante TLS con un certificado proporcionado:</span><span class="sxs-lookup"><span data-stu-id="d645a-365">Connections are secured by TLS with a supplied certificate:</span></span>

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
        });
```

<span data-ttu-id="d645a-366">Opcionalmente, cree una implementación `IConnectionAdapter` para filtrar los protocolos de enlace TLS en una base por conexión para los cifrados específicos:</span><span class="sxs-lookup"><span data-stu-id="d645a-366">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        });
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

<span data-ttu-id="d645a-367">*Defina el protocolo a partir de la configuración*</span><span class="sxs-lookup"><span data-stu-id="d645a-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="d645a-368">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d645a-368">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="d645a-369">En el siguiente ejemplo de *appsettings.json*, se establece un protocolo de conexión predeterminado (HTTP/1.1 y HTTP/2) para todos los puntos de conexión de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="d645a-369">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="d645a-370">El siguiente ejemplo de archivo de configuración establece un protocolo de conexión para un punto de conexión específico:</span><span class="sxs-lookup"><span data-stu-id="d645a-370">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="d645a-371">Los protocolos especificados en el código invalidan los valores establecidos por la configuración.</span><span class="sxs-lookup"><span data-stu-id="d645a-371">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

## <a name="transport-configuration"></a><span data-ttu-id="d645a-372">Configuración de transporte</span><span class="sxs-lookup"><span data-stu-id="d645a-372">Transport configuration</span></span>

<span data-ttu-id="d645a-373">Desde el lanzamiento de ASP.NET Core 2.1, el transporte predeterminado de Kestrel deja de basarse en Libuv y pasa a basarse en sockets administrados.</span><span class="sxs-lookup"><span data-stu-id="d645a-373">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="d645a-374">Se trata de un cambio muy importante para las aplicaciones ASP.NET Core 2.0 que actualizan a 2.1 y llaman a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv), y que dependen de cualquiera de los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="d645a-374">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="d645a-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referencia de paquete directa)</span><span class="sxs-lookup"><span data-stu-id="d645a-375">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="d645a-376">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="d645a-376">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="d645a-377">En el caso de los proyectos de ASP.NET Core 2.1 o posterior que usan el metapaquete [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) y requieren el uso de Libuv:</span><span class="sxs-lookup"><span data-stu-id="d645a-377">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="d645a-378">Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d645a-378">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="d645a-379">Llame a [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="d645a-379">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="d645a-380">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="d645a-380">URL prefixes</span></span>

<span data-ttu-id="d645a-381">Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="d645a-381">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="d645a-382">Solo son válidos los prefijos de dirección URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="d645a-382">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="d645a-383">Kestrel no admite HTTPS al configurar enlaces de dirección URL con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d645a-383">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="d645a-384">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="d645a-384">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="d645a-385">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="d645a-385">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="d645a-386">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="d645a-386">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="d645a-387">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="d645a-387">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="d645a-388">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="d645a-388">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="d645a-389">Los nombres de host, `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="d645a-389">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="d645a-390">Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="d645a-390">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="d645a-391">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="d645a-391">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="d645a-392">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="d645a-392">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="d645a-393">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="d645a-393">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="d645a-394">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d645a-394">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="d645a-395">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="d645a-395">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="d645a-396">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="d645a-396">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="d645a-397">Filtrado de hosts</span><span class="sxs-lookup"><span data-stu-id="d645a-397">Host filtering</span></span>

<span data-ttu-id="d645a-398">Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host.</span><span class="sxs-lookup"><span data-stu-id="d645a-398">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="d645a-399">El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido.</span><span class="sxs-lookup"><span data-stu-id="d645a-399">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="d645a-400">Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas.</span><span class="sxs-lookup"><span data-stu-id="d645a-400">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="d645a-401">Los encabezados `Host` no están validados.</span><span class="sxs-lookup"><span data-stu-id="d645a-401">`Host` headers aren't validated.</span></span>

<span data-ttu-id="d645a-402">Como solución alternativa, use el Middleware de filtrado de hosts.</span><span class="sxs-lookup"><span data-stu-id="d645a-402">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="d645a-403">El Middleware de filtrado de hosts se facilita a través del paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior).</span><span class="sxs-lookup"><span data-stu-id="d645a-403">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="d645a-404">Este middleware se agrega por medio de [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), que llama a [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="d645a-404">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="d645a-405">El Middleware de filtrado de hosts está deshabilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d645a-405">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="d645a-406">Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="d645a-406">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="d645a-407">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="d645a-407">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="d645a-408">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d645a-408">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="d645a-409">El [Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="d645a-409">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="d645a-410">El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios.</span><span class="sxs-lookup"><span data-stu-id="d645a-410">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="d645a-411">Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado `Host` no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga.</span><span class="sxs-lookup"><span data-stu-id="d645a-411">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="d645a-412">Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral de acceso público o cuando el encabezado `Host` se reenvía directamente.</span><span class="sxs-lookup"><span data-stu-id="d645a-412">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="d645a-413">Para obtener más información sobre el Middleware de encabezados reenviados, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d645a-413">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d645a-414">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d645a-414">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="d645a-415">Código fuente de Kestrel</span><span class="sxs-lookup"><span data-stu-id="d645a-415">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* <span data-ttu-id="d645a-416">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Enrutamiento y sintaxis de mensajes [Sección 5.4: Host])</span><span class="sxs-lookup"><span data-stu-id="d645a-416">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
