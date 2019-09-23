---
title: Implementación del servidor web Kestrel en ASP.NET Core
author: guardrex
description: Obtenga información sobre Kestrel, el servidor web multiplataforma de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011162"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fd8e0-103">Implementación del servidor web Kestrel en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd8e0-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fd8e0-104">Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fd8e0-105">Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fd8e0-106">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fd8e0-107">Kestrel admite los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fd8e0-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-108">HTTPS</span></span>
* <span data-ttu-id="fd8e0-109">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fd8e0-110">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="fd8e0-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="fd8e0-111">HTTP/2 (excepto en macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="fd8e0-112">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="fd8e0-113">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fd8e0-114">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd8e0-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="fd8e0-115">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="fd8e0-115">HTTP/2 support</span></span>

<span data-ttu-id="fd8e0-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) está disponible para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="fd8e0-117">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="fd8e0-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="fd8e0-118">Windows Server 2016/Windows 10 o posterior&Dagger;</span><span class="sxs-lookup"><span data-stu-id="fd8e0-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="fd8e0-119">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="fd8e0-120">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="fd8e0-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="fd8e0-121">Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="fd8e0-122">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="fd8e0-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fd8e0-123">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="fd8e0-124">&Dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="fd8e0-125">La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="fd8e0-126">Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="fd8e0-127">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="fd8e0-128">HTTP/2 está deshabilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="fd8e0-129">Para obtener más información sobre la configuración, consulte las secciones [Opciones de Kestrel](#kestrel-options) y [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fd8e0-130">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="fd8e0-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fd8e0-131">Kestrel se puede usar por sí solo o con un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fd8e0-132">Un servidor proxy inverso recibe las solicitudes HTTP de la red y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fd8e0-133">Kestrel empleado como servidor web perimetral (accesible desde Internet):</span><span class="sxs-lookup"><span data-stu-id="fd8e0-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fd8e0-135">Kestrel empleado en una configuración de proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd8e0-137">Cualquiera de las configuraciones, con o sin servidor proxy inverso, es una configuración de hospedaje admitida.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fd8e0-138">Kestrel, utilizado como un servidor perimetral sin un servidor proxy inverso, no permite compartir la misma dirección IP y el mismo puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fd8e0-139">Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente de los encabezados `Host` de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fd8e0-140">Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fd8e0-141">Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fd8e0-142">Un proxy inverso puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-142">A reverse proxy:</span></span>

* <span data-ttu-id="fd8e0-143">Limitar el área expuesta públicamente de las aplicaciones que hospeda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fd8e0-144">Proporcionar una capa extra de configuración y defensa.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fd8e0-145">Posiblemente, integrarse mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fd8e0-146">Simplificar el equilibrio de carga y la configuración de una comunicación segura (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fd8e0-147">Solamente el servidor proxy inverso necesita un certificado X.509, y dicho servidor se puede comunicar con los servidores de la aplicación en la red interna por medio de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-148">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fd8e0-149">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd8e0-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fd8e0-150">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fd8e0-151">En *Program.cs*, el código de plantilla llama a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, que a su vez llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="fd8e0-152">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder` y `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="fd8e0-153">Si la aplicación no llama a `CreateDefaultBuilder` para configurar el host, llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **antes** de llamar a `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="fd8e0-154">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd8e0-154">Kestrel options</span></span>

<span data-ttu-id="fd8e0-155">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fd8e0-156">Establezca restricciones en la propiedad <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fd8e0-157">La propiedad `Limits` contiene una instancia de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fd8e0-158">En los ejemplos siguientes se usa el espacio de nombres <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="fd8e0-159">Tiempo de expiración de la conexión persistente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fd8e0-160">Obtiene o establece el [tiempo de expiración de la conexión persistente](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fd8e0-161">El valor predeterminado es de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="fd8e0-162">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fd8e0-163">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="fd8e0-164">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fd8e0-165">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="fd8e0-166">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fd8e0-167">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fd8e0-168">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fd8e0-169">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fd8e0-170">Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="fd8e0-171">Invalide la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fd8e0-172">Se inicia una excepción si la aplicación configura el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fd8e0-173">Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fd8e0-174">Cuando se ejecuta una aplicación [fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model) detrás del [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module), el límite de tamaño del cuerpo de la solicitud de Kestrel se deshabilita porque IIS ya establece el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fd8e0-175">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fd8e0-176">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fd8e0-177">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fd8e0-178">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fd8e0-179">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fd8e0-180">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fd8e0-181">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fd8e0-182">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="fd8e0-183">Invalide los límites de velocidad mínima por solicitud en el middleware:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="fd8e0-184">La característica <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> a la que se hace referencia en el ejemplo anterior no está presente en `HttpContext.Features` para las solicitudes HTTP/2, porque generalmente no se permite modificar los límites de velocidad por solicitud debido a la compatibilidad del protocolo con la multiplexación de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="fd8e0-185">Sin embargo, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> sigue estando presente en `HttpContext.Features` para las solicitudes HTTP/2, ya que el límite de velocidad de lectura aún puede estar *completamente deshabilitado* por solicitud estableciendo `IHttpMinRequestBodyDataRateFeature.MinDataRate` en `null` incluso para una solicitud HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="fd8e0-186">Si se intenta leer `IHttpMinRequestBodyDataRateFeature.MinDataRate` o se intenta su establecimiento en un valor distinto de `null`, se obtendrá una excepción `NotSupportedException` con una solicitud HTTP/2 dada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="fd8e0-187">Se siguen aplicando límites de velocidad en todo el servidor configurados con `KestrelServerOptions.Limits` a las conexiones HTTP/1.x y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="fd8e0-188">Tiempo de expiración de los encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fd8e0-189">Obtiene o establece la cantidad máxima de tiempo que el servidor pasa recibiendo las cabeceras de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fd8e0-190">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="fd8e0-191">Secuencias máximas por conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-191">Maximum streams per connection</span></span>

<span data-ttu-id="fd8e0-192">`Http2.MaxStreamsPerConnection` limita el número de secuencias de solicitudes simultáneas por conexión HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="fd8e0-193">Se rechazarán las secuencias en exceso.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="fd8e0-194">El valor predeterminado es 100.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="fd8e0-195">Tamaño de la tabla de encabezado</span><span class="sxs-lookup"><span data-stu-id="fd8e0-195">Header table size</span></span>

<span data-ttu-id="fd8e0-196">El descodificador HPACK descomprime los encabezados HTTP para las conexiones HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="fd8e0-197">`Http2.HeaderTableSize` limita el tamaño de la tabla de compresión de encabezado que usa el descodificador HPACK.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="fd8e0-198">El valor se proporciona en octetos y debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="fd8e0-199">El valor predeterminado es 4096.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="fd8e0-200">Tamaño máximo de marco</span><span class="sxs-lookup"><span data-stu-id="fd8e0-200">Maximum frame size</span></span>

<span data-ttu-id="fd8e0-201">`Http2.MaxFrameSize` indica el tamaño máximo permitido de una carga de marco de conexión HTTP/2 recibida o enviada por el servidor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="fd8e0-202">El valor se proporciona en octetos y debe estar comprendido entre 2^14 (16 384) y 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="fd8e0-203">El valor predeterminado es 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="fd8e0-204">Tamaño máximo del encabezado de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-204">Maximum request header size</span></span>

<span data-ttu-id="fd8e0-205">`Http2.MaxRequestHeaderFieldSize` indica el tamaño máximo permitido (en octetos) de los valores de los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="fd8e0-206">Este límite se aplica al nombre y al valor en sus representaciones comprimidas y no comprimidas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="fd8e0-207">El valor debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="fd8e0-208">El valor predeterminado es 8192.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="fd8e0-209">Tamaño inicial de la ventana de conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-209">Initial connection window size</span></span>

<span data-ttu-id="fd8e0-210">`Http2.InitialConnectionWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez de forma agregada en todas las solicitudes (transmisiones) por conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="fd8e0-211">Las solicitudes también están limitadas por `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fd8e0-212">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="fd8e0-213">El valor predeterminado es 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="fd8e0-214">Tamaño inicial de la ventana de transmisión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-214">Initial stream window size</span></span>

<span data-ttu-id="fd8e0-215">`Http2.InitialStreamWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez por solicitud (transmisión).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="fd8e0-216">Las solicitudes también están limitadas por `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="fd8e0-217">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="fd8e0-218">El valor predeterminado es 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="fd8e0-219">E/S sincrónica</span><span class="sxs-lookup"><span data-stu-id="fd8e0-219">Synchronous IO</span></span>

<span data-ttu-id="fd8e0-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controla si se permite la E/S sincrónica para la solicitud y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fd8e0-221">El valor predeterminado es `false`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-222">Un gran número de operaciones de E/S sincrónicas de bloqueo puede dar lugar al colapso del grupo de subprocesos, lo que hace que la aplicación no responda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fd8e0-223">Habilite solo `AllowSynchronousIO` al usar una biblioteca que no admite la E/S asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fd8e0-224">En el siguiente ejemplo se habilita la E/S sincrónica:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="fd8e0-225">Para más información sobre otras opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fd8e0-226">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-226">Endpoint configuration</span></span>

<span data-ttu-id="fd8e0-227">ASP.NET Core enlaza de forma predeterminada a:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fd8e0-228">`https://localhost:5001` (cuando hay presente un certificado de desarrollo local)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fd8e0-229">Especifique direcciones URL mediante los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-229">Specify URLs using the:</span></span>

* <span data-ttu-id="fd8e0-230">La variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fd8e0-231">El argumento de la línea de comandos `--urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fd8e0-232">La clave de configuración de host `urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="fd8e0-233">El método de extensión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="fd8e0-234">El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fd8e0-235">Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fd8e0-236">Para más información sobre estos enfoques, consulte [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fd8e0-237">Un certificado de desarrollo se crea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-237">A development certificate is created:</span></span>

* <span data-ttu-id="fd8e0-238">Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fd8e0-239">Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fd8e0-240">Algunos exploradores necesitan que se conceda permiso explícito para confiar en el certificado de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fd8e0-241">Las plantillas de proyecto configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fd8e0-242">Llame a los métodos <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> o <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> de <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> para configurar los puertos y los prefijos de dirección URL para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fd8e0-243">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fd8e0-244">Configuración de `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fd8e0-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fd8e0-246">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fd8e0-247">Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fd8e0-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fd8e0-249">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fd8e0-250">Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="fd8e0-251">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="fd8e0-252">Crea un cargador de configuración para configurar Kestrel que toma una <xref:Microsoft.Extensions.Configuration.IConfiguration> como entrada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fd8e0-253">El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fd8e0-254">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="fd8e0-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fd8e0-255">Configure Kestrel para que use HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fd8e0-256">Extensiones de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fd8e0-257">`UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fd8e0-258">Produce una excepción si no hay ningún certificado predeterminado configurado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-258">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fd8e0-259">Parámetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fd8e0-260">`filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fd8e0-261">`password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fd8e0-262">`configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fd8e0-263">Devuelve `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fd8e0-264">`storeName` es el almacén de certificados desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fd8e0-265">`subject` es el nombre del sujeto del certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fd8e0-266">`allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fd8e0-267">`location` es la ubicación del almacén desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fd8e0-268">`serverCertificate` es el certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fd8e0-269">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fd8e0-270">Como mínimo, debe existir un certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fd8e0-271">Estas son las configuraciones compatibles:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-271">Supported configurations described next:</span></span>

* <span data-ttu-id="fd8e0-272">Sin configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-272">No configuration</span></span>
* <span data-ttu-id="fd8e0-273">Reemplazar el certificado predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fd8e0-274">Cambiar los valores predeterminados en el código</span><span class="sxs-lookup"><span data-stu-id="fd8e0-274">Change the defaults in code</span></span>

<span data-ttu-id="fd8e0-275">*Sin configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-275">*No configuration*</span></span>

<span data-ttu-id="fd8e0-276">Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fd8e0-277">*Reemplazar el certificado predeterminado de configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fd8e0-278">`CreateDefaultBuilder` llama a `Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fd8e0-279">Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fd8e0-280">Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fd8e0-281">En el siguiente ejemplo de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fd8e0-282">Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fd8e0-283">Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="fd8e0-284">Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fd8e0-285">Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fd8e0-286">Notas sobre el esquema:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-286">Schema notes:</span></span>

* <span data-ttu-id="fd8e0-287">En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fd8e0-288">Por ejemplo, `HTTPS` y `Https` son válidos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fd8e0-289">El parámetro `Url` es necesario en cada punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fd8e0-290">El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fd8e0-291">En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fd8e0-292">Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fd8e0-293">La sección `Certificate` es opcional.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="fd8e0-294">Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fd8e0-295">Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fd8e0-296">La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fd8e0-297">Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fd8e0-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, listenOptions => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="fd8e0-299">Se puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para seguir con la iteración en el cargador existente, como el proporcionado por <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fd8e0-300">La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fd8e0-301">Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("{SECTION}"))` con otra sección.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fd8e0-302">Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fd8e0-303">El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fd8e0-304">`KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fd8e0-305">En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fd8e0-306">*Cambiar los valores predeterminados en el código*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-306">*Change the defaults in code*</span></span>

<span data-ttu-id="fd8e0-307">`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fd8e0-308">Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });

                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    listenOptions.SslProtocols = SslProtocols.Tls12;
                });
            });
        });
```

<span data-ttu-id="fd8e0-309">*Compatibilidad de Kestrel con SNI*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fd8e0-310">[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fd8e0-311">Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fd8e0-312">El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fd8e0-313">Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fd8e0-314">La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fd8e0-315">La compatibilidad con SNI requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-315">SNI support requires:</span></span>

* <span data-ttu-id="fd8e0-316">Ejecutarse en el marco de destino `netcoreapp2.1` o posterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fd8e0-317">En `net461` o posterior, se invoca la devolución de llamada, pero `name` siempre es `null`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fd8e0-318">`name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fd8e0-319">Todos los sitios web deben ejecutarse en la misma instancia de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fd8e0-320">Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ListenAnyIP(5005, listenOptions =>
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
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fd8e0-321">Enlazar a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="fd8e0-321">Bind to a TCP socket</span></span>

<span data-ttu-id="fd8e0-322">El método <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se enlaza a un socket TCP y una expresión lambda de opciones permite configurar un certificado X.509:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="fd8e0-323">En el ejemplo se configura HTTPS para un punto de conexión con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fd8e0-324">Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fd8e0-325">Enlazar a un socket de Unix</span><span class="sxs-lookup"><span data-stu-id="fd8e0-325">Bind to a Unix socket</span></span>

<span data-ttu-id="fd8e0-326">Escuche en un socket de Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="fd8e0-327">Puerto 0</span><span class="sxs-lookup"><span data-stu-id="fd8e0-327">Port 0</span></span>

<span data-ttu-id="fd8e0-328">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fd8e0-329">En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fd8e0-330">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fd8e0-331">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="fd8e0-331">Limitations</span></span>

<span data-ttu-id="fd8e0-332">Configure puntos de conexión con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fd8e0-333">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="fd8e0-334">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-334">`urls` host configuration key</span></span>
* <span data-ttu-id="fd8e0-335">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fd8e0-336">Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fd8e0-337">Sin embargo, tenga en cuenta las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fd8e0-338">HTTPS no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fd8e0-339">Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fd8e0-340">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-340">IIS endpoint configuration</span></span>

<span data-ttu-id="fd8e0-341">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fd8e0-342">Para más información, vea el tema [Módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="fd8e0-343">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="fd8e0-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="fd8e0-344">La propiedad `Protocols` establece los protocolos HTTP (`HttpProtocols`) habilitados en un punto de conexión o para el servidor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="fd8e0-345">Asigne un valor a la propiedad `Protocols` desde el valor de enumeración `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="fd8e0-346">Valor de enumeración `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="fd8e0-347">Protocolo de conexión permitido</span><span class="sxs-lookup"><span data-stu-id="fd8e0-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="fd8e0-348">HTTP/1.1 solo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-348">HTTP/1.1 only.</span></span> <span data-ttu-id="fd8e0-349">Puede usarse con o sin TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="fd8e0-350">HTTP/2 solo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-350">HTTP/2 only.</span></span> <span data-ttu-id="fd8e0-351">Se pueden utilizar sin TLS solo si el cliente admite un [modo de conocimientos previos](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="fd8e0-352">HTTP/1.1 y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="fd8e0-353">HTTP/2 necesita que el cliente seleccione HTTP/2 en el protocolo de enlace [Negociación de protocolo de nivel de aplicación (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) de TLS; en caso contrario, el valor predeterminado de la conexión es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="fd8e0-354">El valor `ListenOptions.Protocols` predeterminado de cualquier punto de conexión es `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="fd8e0-355">Restricciones de TLS para HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="fd8e0-356">TLS 1.2 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="fd8e0-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="fd8e0-357">Renegociación deshabilitada</span><span class="sxs-lookup"><span data-stu-id="fd8e0-357">Renegotiation disabled</span></span>
* <span data-ttu-id="fd8e0-358">Compresión deshabilitada</span><span class="sxs-lookup"><span data-stu-id="fd8e0-358">Compression disabled</span></span>
* <span data-ttu-id="fd8e0-359">Tamaños de intercambio de claves efímeras mínimos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="fd8e0-360">Curva elíptica Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;; 224 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="fd8e0-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="fd8e0-361">Campo finito Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;; 2048 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="fd8e0-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="fd8e0-362">Conjunto de cifrado no restringido</span><span class="sxs-lookup"><span data-stu-id="fd8e0-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="fd8e0-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con la curva elíptica P-256 &lbrack;`FIPS186`&rbrack; es compatible de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="fd8e0-364">El siguiente ejemplo permite conexiones HTTP/1.1 y HTTP/2 en el puerto 8000.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="fd8e0-365">Las conexiones se protegen mediante TLS con un certificado proporcionado:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="fd8e0-366">Opcionalmente, use el middleware de conexión para filtrar los protocolos de enlace TLS en una base por conexión para los cifrados específicos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="fd8e0-367">*Defina el protocolo a partir de la configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="fd8e0-368">`CreateDefaultBuilder` llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="fd8e0-369">En el siguiente ejemplo de *appsettings.json*, se establece HTTP/1.1 como el protocolo de conexión predeterminado para todos los puntos de conexión:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="fd8e0-370">En el siguiente ejemplo de *appsettings.json* se establece el protocolo de conexión HTTP/1.1 para un punto de conexión específico:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="fd8e0-371">Los protocolos especificados en el código invalidan los valores establecidos por la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fd8e0-372">Configuración de transporte</span><span class="sxs-lookup"><span data-stu-id="fd8e0-372">Transport configuration</span></span>

<span data-ttu-id="fd8e0-373">Para los proyectos que necesitan el uso de Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="fd8e0-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="fd8e0-374">Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="fd8e0-375">Llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> en `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="fd8e0-376">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="fd8e0-376">URL prefixes</span></span>

<span data-ttu-id="fd8e0-377">Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fd8e0-378">Solo son válidos los prefijos de dirección URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fd8e0-379">Kestrel no admite HTTPS al configurar enlaces de dirección URL con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fd8e0-380">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fd8e0-381">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fd8e0-382">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fd8e0-383">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fd8e0-384">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fd8e0-385">Los nombres de host, `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fd8e0-386">Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fd8e0-387">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fd8e0-388">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fd8e0-389">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fd8e0-390">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fd8e0-391">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fd8e0-392">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fd8e0-393">Filtrado de hosts</span><span class="sxs-lookup"><span data-stu-id="fd8e0-393">Host filtering</span></span>

<span data-ttu-id="fd8e0-394">Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fd8e0-395">El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fd8e0-396">Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fd8e0-397">Los encabezados `Host` no están validados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fd8e0-398">Como solución alternativa, use el Middleware de filtrado de hosts.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fd8e0-399">El middleware de filtrado de host lo proporciona el paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se proporciona implícitamente para aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="fd8e0-400">El middleware se agrega por medio de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, que llama a <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fd8e0-401">El Middleware de filtrado de hosts está deshabilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fd8e0-402">Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fd8e0-403">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fd8e0-404">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fd8e0-405">[Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fd8e0-406">El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fd8e0-407">Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado `Host` no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fd8e0-408">Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral de acceso público o cuando el encabezado `Host` se reenvía directamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fd8e0-409">Para obtener más información sobre el Middleware de encabezados reenviados, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fd8e0-410">Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fd8e0-411">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fd8e0-412">Kestrel admite los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fd8e0-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-413">HTTPS</span></span>
* <span data-ttu-id="fd8e0-414">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fd8e0-415">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="fd8e0-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="fd8e0-416">HTTP/2 (excepto en macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="fd8e0-417">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="fd8e0-418">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fd8e0-419">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd8e0-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="fd8e0-420">Compatibilidad con HTTP/2</span><span class="sxs-lookup"><span data-stu-id="fd8e0-420">HTTP/2 support</span></span>

<span data-ttu-id="fd8e0-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) está disponible para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="fd8e0-422">Sistema operativo&dagger;</span><span class="sxs-lookup"><span data-stu-id="fd8e0-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="fd8e0-423">Windows Server 2016/Windows 10 o posterior&Dagger;</span><span class="sxs-lookup"><span data-stu-id="fd8e0-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="fd8e0-424">Linux con OpenSSL 1.0.2 o posterior (por ejemplo, Ubuntu 16.04 o posterior)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="fd8e0-425">Plataforma de destino: .NET Core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="fd8e0-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="fd8e0-426">Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="fd8e0-427">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="fd8e0-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fd8e0-428">&dagger;HTTP/2 se admitirá en una versión futura en macOS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="fd8e0-429">&Dagger;Kestrel tiene compatibilidad limitada para HTTP/2 en Windows Server 2012 R2 y Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="fd8e0-430">La compatibilidad es limitada porque la lista de conjuntos de cifrado TLS admitidos y disponibles en estos sistemas operativos está limitada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="fd8e0-431">Se puede requerir un certificado generado mediante Elliptic Curve Digital Signature Algorithm (ECDSA) para proteger las conexiones TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="fd8e0-432">Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="fd8e0-433">HTTP/2 está deshabilitado de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="fd8e0-434">Para obtener más información sobre la configuración, consulte las secciones [Opciones de Kestrel](#kestrel-options) y [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fd8e0-435">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="fd8e0-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fd8e0-436">Kestrel se puede usar por sí solo o con un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fd8e0-437">Un servidor proxy inverso recibe las solicitudes HTTP de la red y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fd8e0-438">Kestrel empleado como servidor web perimetral (accesible desde Internet):</span><span class="sxs-lookup"><span data-stu-id="fd8e0-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fd8e0-440">Kestrel empleado en una configuración de proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd8e0-442">Cualquiera de las configuraciones, con o sin servidor proxy inverso, es una configuración de hospedaje admitida.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fd8e0-443">Kestrel, utilizado como un servidor perimetral sin un servidor proxy inverso, no permite compartir la misma dirección IP y el mismo puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fd8e0-444">Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente de los encabezados `Host` de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fd8e0-445">Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fd8e0-446">Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fd8e0-447">Un proxy inverso puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-447">A reverse proxy:</span></span>

* <span data-ttu-id="fd8e0-448">Limitar el área expuesta públicamente de las aplicaciones que hospeda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fd8e0-449">Proporcionar una capa extra de configuración y defensa.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fd8e0-450">Posiblemente, integrarse mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fd8e0-451">Simplificar el equilibrio de carga y la configuración de una comunicación segura (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fd8e0-452">Solamente el servidor proxy inverso necesita un certificado X.509 y dicho servidor se puede comunicar con los servidores de la aplicación en la red interna por medio de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-453">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fd8e0-454">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd8e0-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fd8e0-455">El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fd8e0-456">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fd8e0-457">En *Program.cs*, el código de plantilla llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, que a su vez llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="fd8e0-458">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, use `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="fd8e0-459">Si la aplicación no llama a `CreateDefaultBuilder` para configurar el host, llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **antes** de llamar a `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="fd8e0-460">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd8e0-460">Kestrel options</span></span>

<span data-ttu-id="fd8e0-461">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fd8e0-462">Establezca restricciones en la propiedad <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fd8e0-463">La propiedad `Limits` contiene una instancia de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fd8e0-464">En los ejemplos siguientes se usa el espacio de nombres <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="fd8e0-465">Tiempo de expiración de la conexión persistente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fd8e0-466">Obtiene o establece el [tiempo de expiración de la conexión persistente](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fd8e0-467">El valor predeterminado es de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="fd8e0-468">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fd8e0-469">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="fd8e0-470">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fd8e0-471">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="fd8e0-472">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fd8e0-473">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fd8e0-474">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fd8e0-475">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fd8e0-476">Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="fd8e0-477">Invalide la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fd8e0-478">Se inicia una excepción si la aplicación configura el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fd8e0-479">Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fd8e0-480">Cuando se ejecuta una aplicación [fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model) detrás del [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module), el límite de tamaño del cuerpo de la solicitud de Kestrel se deshabilita porque IIS ya establece el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fd8e0-481">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fd8e0-482">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fd8e0-483">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fd8e0-484">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fd8e0-485">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fd8e0-486">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fd8e0-487">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fd8e0-488">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="fd8e0-489">Invalide los límites de velocidad mínima por solicitud en el middleware:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="fd8e0-490">Ninguna de las características de velocidad a las que se hace referencia en el ejemplo anterior están presentes en `HttpContext.Features` para las solicitudes HTTP/2, porque no se permite modificar los límites de velocidad por solicitud en HTTP/2 debido a la compatibilidad del protocolo con la multiplexación de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="fd8e0-491">Se siguen aplicando límites de velocidad en todo el servidor configurados con `KestrelServerOptions.Limits` a las conexiones HTTP/1.x y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="fd8e0-492">Tiempo de expiración de los encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fd8e0-493">Obtiene o establece la cantidad máxima de tiempo que el servidor pasa recibiendo las cabeceras de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fd8e0-494">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="fd8e0-495">Secuencias máximas por conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-495">Maximum streams per connection</span></span>

<span data-ttu-id="fd8e0-496">`Http2.MaxStreamsPerConnection` limita el número de secuencias de solicitudes simultáneas por conexión HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="fd8e0-497">Se rechazarán las secuencias en exceso.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="fd8e0-498">El valor predeterminado es 100.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="fd8e0-499">Tamaño de la tabla de encabezado</span><span class="sxs-lookup"><span data-stu-id="fd8e0-499">Header table size</span></span>

<span data-ttu-id="fd8e0-500">El descodificador HPACK descomprime los encabezados HTTP para las conexiones HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="fd8e0-501">`Http2.HeaderTableSize` limita el tamaño de la tabla de compresión de encabezado que usa el descodificador HPACK.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="fd8e0-502">El valor se proporciona en octetos y debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="fd8e0-503">El valor predeterminado es 4096.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="fd8e0-504">Tamaño máximo de marco</span><span class="sxs-lookup"><span data-stu-id="fd8e0-504">Maximum frame size</span></span>

<span data-ttu-id="fd8e0-505">`Http2.MaxFrameSize` indica el tamaño máximo de la carga útil de la trama de conexión HTTP/2 que se puede recibir.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="fd8e0-506">El valor se proporciona en octetos y debe estar comprendido entre 2^14 (16 384) y 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="fd8e0-507">El valor predeterminado es 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="fd8e0-508">Tamaño máximo del encabezado de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-508">Maximum request header size</span></span>

<span data-ttu-id="fd8e0-509">`Http2.MaxRequestHeaderFieldSize` indica el tamaño máximo permitido (en octetos) de los valores de los encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="fd8e0-510">Este límite se aplica al nombre y al valor conjuntamente en sus representaciones comprimidas y no comprimidas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="fd8e0-511">El valor debe ser mayor que cero (0).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="fd8e0-512">El valor predeterminado es 8192.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="fd8e0-513">Tamaño inicial de la ventana de conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-513">Initial connection window size</span></span>

<span data-ttu-id="fd8e0-514">`Http2.InitialConnectionWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez de forma agregada en todas las solicitudes (transmisiones) por conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="fd8e0-515">Las solicitudes también están limitadas por `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fd8e0-516">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="fd8e0-517">El valor predeterminado es 128 KB (131 072).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="fd8e0-518">Tamaño inicial de la ventana de transmisión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-518">Initial stream window size</span></span>

<span data-ttu-id="fd8e0-519">`Http2.InitialStreamWindowSize` indica la cantidad máxima de datos del cuerpo de solicitud (en bytes) que el servidor almacena en búfer a la vez por solicitud (transmisión).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="fd8e0-520">Las solicitudes también están limitadas por `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fd8e0-521">El valor debe ser igual o mayor que 65 535 y menor que 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="fd8e0-522">El valor predeterminado es 96 KB (98 304).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="fd8e0-523">E/S sincrónica</span><span class="sxs-lookup"><span data-stu-id="fd8e0-523">Synchronous IO</span></span>

<span data-ttu-id="fd8e0-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controla si se permite la E/S sincrónica para la solicitud y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fd8e0-525">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-526">Un gran número de operaciones de E/S sincrónicas de bloqueo puede dar lugar al colapso del grupo de subprocesos, lo que hace que la aplicación no responda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fd8e0-527">Habilite solo `AllowSynchronousIO` al usar una biblioteca que no admite la E/S asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fd8e0-528">En el siguiente ejemplo se habilita la E/S sincrónica:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="fd8e0-529">Para más información sobre otras opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fd8e0-530">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-530">Endpoint configuration</span></span>

<span data-ttu-id="fd8e0-531">ASP.NET Core enlaza de forma predeterminada a:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fd8e0-532">`https://localhost:5001` (cuando hay presente un certificado de desarrollo local)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fd8e0-533">Especifique direcciones URL mediante los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-533">Specify URLs using the:</span></span>

* <span data-ttu-id="fd8e0-534">La variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fd8e0-535">El argumento de la línea de comandos `--urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fd8e0-536">La clave de configuración de host `urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="fd8e0-537">El método de extensión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="fd8e0-538">El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fd8e0-539">Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fd8e0-540">Para más información sobre estos enfoques, consulte [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fd8e0-541">Un certificado de desarrollo se crea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-541">A development certificate is created:</span></span>

* <span data-ttu-id="fd8e0-542">Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fd8e0-543">Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fd8e0-544">Algunos exploradores necesitan que se conceda permiso explícito para confiar en el certificado de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fd8e0-545">Las plantillas de proyecto configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fd8e0-546">Llame a los métodos <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> o <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> de <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> para configurar los puertos y los prefijos de dirección URL para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fd8e0-547">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fd8e0-548">Configuración de `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fd8e0-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fd8e0-550">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fd8e0-551">Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fd8e0-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fd8e0-553">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fd8e0-554">Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="fd8e0-555">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="fd8e0-556">Crea un cargador de configuración para configurar Kestrel que toma una <xref:Microsoft.Extensions.Configuration.IConfiguration> como entrada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fd8e0-557">El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fd8e0-558">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="fd8e0-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fd8e0-559">Configure Kestrel para que use HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fd8e0-560">Extensiones de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fd8e0-561">`UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fd8e0-562">Produce una excepción si no hay ningún certificado predeterminado configurado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-562">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fd8e0-563">Parámetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fd8e0-564">`filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fd8e0-565">`password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fd8e0-566">`configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fd8e0-567">Devuelve `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fd8e0-568">`storeName` es el almacén de certificados desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fd8e0-569">`subject` es el nombre del sujeto del certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fd8e0-570">`allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fd8e0-571">`location` es la ubicación del almacén desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fd8e0-572">`serverCertificate` es el certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fd8e0-573">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fd8e0-574">Como mínimo, debe existir un certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fd8e0-575">Estas son las configuraciones compatibles:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-575">Supported configurations described next:</span></span>

* <span data-ttu-id="fd8e0-576">Sin configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-576">No configuration</span></span>
* <span data-ttu-id="fd8e0-577">Reemplazar el certificado predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fd8e0-578">Cambiar los valores predeterminados en el código</span><span class="sxs-lookup"><span data-stu-id="fd8e0-578">Change the defaults in code</span></span>

<span data-ttu-id="fd8e0-579">*Sin configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-579">*No configuration*</span></span>

<span data-ttu-id="fd8e0-580">Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fd8e0-581">*Reemplazar el certificado predeterminado de configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fd8e0-582">`CreateDefaultBuilder` llama a `Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fd8e0-583">Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fd8e0-584">Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fd8e0-585">En el siguiente ejemplo de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fd8e0-586">Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fd8e0-587">Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="fd8e0-588">Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fd8e0-589">Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fd8e0-590">Notas sobre el esquema:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-590">Schema notes:</span></span>

* <span data-ttu-id="fd8e0-591">En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fd8e0-592">Por ejemplo, `HTTPS` y `Https` son válidos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fd8e0-593">El parámetro `Url` es necesario en cada punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fd8e0-594">El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fd8e0-595">En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fd8e0-596">Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fd8e0-597">La sección `Certificate` es opcional.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="fd8e0-598">Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fd8e0-599">Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fd8e0-600">La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fd8e0-601">Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fd8e0-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, listenOptions => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="fd8e0-603">Se puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para seguir con la iteración en el cargador existente, como el proporcionado por <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fd8e0-604">La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fd8e0-605">Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("{SECTION}"))` con otra sección.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fd8e0-606">Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fd8e0-607">El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fd8e0-608">`KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fd8e0-609">En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fd8e0-610">*Cambiar los valores predeterminados en el código*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-610">*Change the defaults in code*</span></span>

<span data-ttu-id="fd8e0-611">`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fd8e0-612">Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="fd8e0-613">*Compatibilidad de Kestrel con SNI*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fd8e0-614">[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fd8e0-615">Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fd8e0-616">El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fd8e0-617">Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fd8e0-618">La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fd8e0-619">La compatibilidad con SNI requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-619">SNI support requires:</span></span>

* <span data-ttu-id="fd8e0-620">Ejecutarse en el marco de destino `netcoreapp2.1` o posterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fd8e0-621">En `net461` o posterior, se invoca la devolución de llamada, pero `name` siempre es `null`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fd8e0-622">`name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fd8e0-623">Todos los sitios web deben ejecutarse en la misma instancia de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fd8e0-624">Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fd8e0-625">Enlazar a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="fd8e0-625">Bind to a TCP socket</span></span>

<span data-ttu-id="fd8e0-626">El método <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se enlaza a un socket TCP y una expresión lambda de opciones permite configurar un certificado X.509:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="fd8e0-627">En el ejemplo se configura HTTPS para un punto de conexión con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fd8e0-628">Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fd8e0-629">Enlazar a un socket de Unix</span><span class="sxs-lookup"><span data-stu-id="fd8e0-629">Bind to a Unix socket</span></span>

<span data-ttu-id="fd8e0-630">Escuche en un socket de Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="fd8e0-631">Puerto 0</span><span class="sxs-lookup"><span data-stu-id="fd8e0-631">Port 0</span></span>

<span data-ttu-id="fd8e0-632">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fd8e0-633">En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fd8e0-634">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fd8e0-635">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="fd8e0-635">Limitations</span></span>

<span data-ttu-id="fd8e0-636">Configure puntos de conexión con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fd8e0-637">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="fd8e0-638">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-638">`urls` host configuration key</span></span>
* <span data-ttu-id="fd8e0-639">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fd8e0-640">Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fd8e0-641">Sin embargo, tenga en cuenta las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fd8e0-642">HTTPS no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fd8e0-643">Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fd8e0-644">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-644">IIS endpoint configuration</span></span>

<span data-ttu-id="fd8e0-645">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fd8e0-646">Para más información, vea el tema [Módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="fd8e0-647">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="fd8e0-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="fd8e0-648">La propiedad `Protocols` establece los protocolos HTTP (`HttpProtocols`) habilitados en un punto de conexión o para el servidor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="fd8e0-649">Asigne un valor a la propiedad `Protocols` desde el valor de enumeración `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="fd8e0-650">Valor de enumeración `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="fd8e0-651">Protocolo de conexión permitido</span><span class="sxs-lookup"><span data-stu-id="fd8e0-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="fd8e0-652">HTTP/1.1 solo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-652">HTTP/1.1 only.</span></span> <span data-ttu-id="fd8e0-653">Puede usarse con o sin TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="fd8e0-654">HTTP/2 solo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-654">HTTP/2 only.</span></span> <span data-ttu-id="fd8e0-655">Se pueden utilizar sin TLS solo si el cliente admite un [modo de conocimientos previos](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="fd8e0-656">HTTP/1.1 y HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="fd8e0-657">HTTP/2 necesita TLS y una conexión de [Negociación de protocolo de nivel de aplicación (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); en caso contrario, la opción predeterminada para la conexión es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="fd8e0-658">El protocolo predeterminado es HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="fd8e0-659">Restricciones de TLS para HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="fd8e0-660">TLS 1.2 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="fd8e0-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="fd8e0-661">Renegociación deshabilitada</span><span class="sxs-lookup"><span data-stu-id="fd8e0-661">Renegotiation disabled</span></span>
* <span data-ttu-id="fd8e0-662">Compresión deshabilitada</span><span class="sxs-lookup"><span data-stu-id="fd8e0-662">Compression disabled</span></span>
* <span data-ttu-id="fd8e0-663">Tamaños de intercambio de claves efímeras mínimos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="fd8e0-664">Curva elíptica Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;; 224 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="fd8e0-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="fd8e0-665">Campo finito Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;; 2048 bits como mínimo</span><span class="sxs-lookup"><span data-stu-id="fd8e0-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="fd8e0-666">Conjunto de cifrado no restringido</span><span class="sxs-lookup"><span data-stu-id="fd8e0-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="fd8e0-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; con la curva elíptica P-256 &lbrack;`FIPS186`&rbrack; es compatible de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="fd8e0-668">El siguiente ejemplo permite conexiones HTTP/1.1 y HTTP/2 en el puerto 8000.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="fd8e0-669">Las conexiones se protegen mediante TLS con un certificado proporcionado:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-669">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="fd8e0-670">Opcionalmente, cree una implementación `IConnectionAdapter` para filtrar los protocolos de enlace TLS en una base por conexión para los cifrados específicos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

<span data-ttu-id="fd8e0-671">*Defina el protocolo a partir de la configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="fd8e0-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> llama a `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="fd8e0-673">En el siguiente ejemplo de *appsettings.json*, se establece un protocolo de conexión predeterminado (HTTP/1.1 y HTTP/2) para todos los puntos de conexión de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="fd8e0-674">El siguiente ejemplo de archivo de configuración establece un protocolo de conexión para un punto de conexión específico:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="fd8e0-675">Los protocolos especificados en el código invalidan los valores establecidos por la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fd8e0-676">Configuración de transporte</span><span class="sxs-lookup"><span data-stu-id="fd8e0-676">Transport configuration</span></span>

<span data-ttu-id="fd8e0-677">Desde el lanzamiento de ASP.NET Core 2.1, el transporte predeterminado de Kestrel deja de basarse en Libuv y pasa a basarse en sockets administrados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="fd8e0-678">Se trata de un cambio muy importante para las aplicaciones ASP.NET Core 2.0 que actualizan a 2.1 y llaman a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>, y que dependen de cualquiera de los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="fd8e0-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referencia de paquete directa)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="fd8e0-680">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="fd8e0-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="fd8e0-681">Para los proyectos que necesitan el uso de Libuv:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="fd8e0-682">Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="fd8e0-683">Llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="fd8e0-684">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="fd8e0-684">URL prefixes</span></span>

<span data-ttu-id="fd8e0-685">Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fd8e0-686">Solo son válidos los prefijos de dirección URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fd8e0-687">Kestrel no admite HTTPS al configurar enlaces de dirección URL con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fd8e0-688">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fd8e0-689">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fd8e0-690">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fd8e0-691">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fd8e0-692">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fd8e0-693">Los nombres de host, `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fd8e0-694">Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fd8e0-695">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fd8e0-696">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fd8e0-697">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fd8e0-698">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fd8e0-699">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fd8e0-700">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fd8e0-701">Filtrado de hosts</span><span class="sxs-lookup"><span data-stu-id="fd8e0-701">Host filtering</span></span>

<span data-ttu-id="fd8e0-702">Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fd8e0-703">El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fd8e0-704">Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fd8e0-705">Los encabezados `Host` no están validados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fd8e0-706">Como solución alternativa, use el Middleware de filtrado de hosts.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fd8e0-707">El middleware de filtrado de host se facilita a través del paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o 2.2).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="fd8e0-708">El middleware se agrega por medio de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, que llama a <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fd8e0-709">El Middleware de filtrado de hosts está deshabilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fd8e0-710">Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fd8e0-711">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fd8e0-712">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fd8e0-713">[Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fd8e0-714">El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fd8e0-715">Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado `Host` no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fd8e0-716">Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral de acceso público o cuando el encabezado `Host` se reenvía directamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fd8e0-717">Para obtener más información sobre el Middleware de encabezados reenviados, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fd8e0-718">Kestrel es un [servidor web multiplataforma de ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fd8e0-719">Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fd8e0-720">Kestrel admite los siguientes escenarios:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fd8e0-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-721">HTTPS</span></span>
* <span data-ttu-id="fd8e0-722">Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fd8e0-723">Sockets de Unix para alto rendimiento detrás de Nginx</span><span class="sxs-lookup"><span data-stu-id="fd8e0-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="fd8e0-724">Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fd8e0-725">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd8e0-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fd8e0-726">Cuándo usar Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="fd8e0-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fd8e0-727">Kestrel se puede usar por sí solo o con un *servidor proxy inverso*, como [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fd8e0-728">Un servidor proxy inverso recibe las solicitudes HTTP de la red y las reenvía a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fd8e0-729">Kestrel empleado como servidor web perimetral (accesible desde Internet):</span><span class="sxs-lookup"><span data-stu-id="fd8e0-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fd8e0-731">Kestrel empleado en una configuración de proxy inverso:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd8e0-733">Cualquiera de las configuraciones, con o sin servidor proxy inverso, es una configuración de hospedaje admitida.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fd8e0-734">Kestrel, utilizado como un servidor perimetral sin un servidor proxy inverso, no permite compartir la misma dirección IP y el mismo puerto entre varios procesos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fd8e0-735">Si Kestrel se configura para escuchar en un puerto, controla todo el tráfico de ese puerto, independientemente de los encabezados `Host` de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fd8e0-736">Un proxy inverso que puede compartir puertos es capaz de reenviar solicitudes a Kestrel en una única dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fd8e0-737">Aunque no sea necesario un servidor proxy inverso, su uso puede ser útil.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fd8e0-738">Un proxy inverso puede hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-738">A reverse proxy:</span></span>

* <span data-ttu-id="fd8e0-739">Limitar el área expuesta públicamente de las aplicaciones que hospeda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fd8e0-740">Proporcionar una capa extra de configuración y defensa.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fd8e0-741">Posiblemente, integrarse mejor con la infraestructura existente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fd8e0-742">Simplificar el equilibrio de carga y la configuración de una comunicación segura (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fd8e0-743">Solamente el servidor proxy inverso necesita un certificado X.509 y dicho servidor se puede comunicar con los servidores de la aplicación en la red interna por medio de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-744">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fd8e0-745">Cómo usar Kestrel en aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd8e0-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fd8e0-746">El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fd8e0-747">Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fd8e0-748">En *Program.cs*, el código de plantilla llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, que a su vez llama a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="fd8e0-749">Para proporcionar configuración adicional después de llamar a `CreateDefaultBuilder`, llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="fd8e0-750">Opciones de Kestrel</span><span class="sxs-lookup"><span data-stu-id="fd8e0-750">Kestrel options</span></span>

<span data-ttu-id="fd8e0-751">El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fd8e0-752">Establezca restricciones en la propiedad <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fd8e0-753">La propiedad `Limits` contiene una instancia de la clase <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fd8e0-754">En los ejemplos siguientes se usa el espacio de nombres <xref:Microsoft.AspNetCore.Server.Kestrel.Core>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="fd8e0-755">Tiempo de expiración de la conexión persistente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fd8e0-756">Obtiene o establece el [tiempo de expiración de la conexión persistente](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fd8e0-757">El valor predeterminado es de 2 minutos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="fd8e0-758">Las conexiones máximas de cliente</span><span class="sxs-lookup"><span data-stu-id="fd8e0-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fd8e0-759">El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="fd8e0-760">Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fd8e0-761">Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="fd8e0-762">El número máximo de conexiones es ilimitado de forma predeterminada (null).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fd8e0-763">El tamaño máximo del cuerpo de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fd8e0-764">El tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fd8e0-765">El método recomendado para invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fd8e0-766">Este es un ejemplo que muestra cómo configurar la restricción en la aplicación y todas las solicitudes:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="fd8e0-767">Invalide la configuración en una solicitud específica de middleware:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fd8e0-768">Se inicia una excepción si la aplicación configura el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fd8e0-769">Hay una propiedad `IsReadOnly` que señala si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fd8e0-770">Cuando se ejecuta una aplicación [fuera de proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model) detrás del [módulo de ASP.NET Core](xref:host-and-deploy/aspnet-core-module), el límite de tamaño del cuerpo de la solicitud de Kestrel se deshabilita porque IIS ya establece el límite.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fd8e0-771">La velocidad mínima de los datos del cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fd8e0-772">Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fd8e0-773">Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fd8e0-774">Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fd8e0-775">La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de cinco segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fd8e0-776">También se aplica una velocidad mínima a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fd8e0-777">El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fd8e0-778">Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="fd8e0-779">Tiempo de expiración de los encabezados de solicitud</span><span class="sxs-lookup"><span data-stu-id="fd8e0-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fd8e0-780">Obtiene o establece la cantidad máxima de tiempo que el servidor pasa recibiendo las cabeceras de las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fd8e0-781">El valor predeterminado es 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="fd8e0-782">E/S sincrónica</span><span class="sxs-lookup"><span data-stu-id="fd8e0-782">Synchronous IO</span></span>

<span data-ttu-id="fd8e0-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controla si se permite la E/S sincrónica para la solicitud y la respuesta.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fd8e0-784">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="fd8e0-785">Un gran número de operaciones de E/S sincrónicas de bloqueo puede dar lugar al colapso del grupo de subprocesos, lo que hace que la aplicación no responda.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fd8e0-786">Habilite solo `AllowSynchronousIO` al usar una biblioteca que no admite la E/S asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fd8e0-787">En el siguiente ejemplo se deshabilita la E/S sincrónica:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="fd8e0-788">Para más información sobre otras opciones y límites de Kestrel, vea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fd8e0-789">Configuración de punto de conexión</span><span class="sxs-lookup"><span data-stu-id="fd8e0-789">Endpoint configuration</span></span>

<span data-ttu-id="fd8e0-790">ASP.NET Core enlaza de forma predeterminada a:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fd8e0-791">`https://localhost:5001` (cuando hay presente un certificado de desarrollo local)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fd8e0-792">Especifique direcciones URL mediante los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-792">Specify URLs using the:</span></span>

* <span data-ttu-id="fd8e0-793">La variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fd8e0-794">El argumento de la línea de comandos `--urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fd8e0-795">La clave de configuración de host `urls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="fd8e0-796">El método de extensión `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="fd8e0-797">El valor que estos métodos suministran puede ser uno o más puntos de conexión HTTP y HTTPS (este último, si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fd8e0-798">Configure el valor como una lista separada por punto y coma (por ejemplo, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fd8e0-799">Para más información sobre estos enfoques, consulte [Direcciones URL del servidor](xref:fundamentals/host/web-host#server-urls) e [Invalidar la configuración](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fd8e0-800">Un certificado de desarrollo se crea:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-800">A development certificate is created:</span></span>

* <span data-ttu-id="fd8e0-801">Cuando el [SDK de .NET Core](/dotnet/core/sdk) está instalado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fd8e0-802">Para crear un certificado, se usa la [herramienta dev-certs](xref:aspnetcore-2.1#https).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fd8e0-803">Algunos exploradores necesitan que se conceda permiso explícito para confiar en el certificado de desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fd8e0-804">Las plantillas de proyecto configuran aplicaciones para que se ejecuten en HTTPS de forma predeterminada e incluyen [redirección de HTTPS y compatibilidad con HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fd8e0-805">Llame a los métodos <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> o <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> de <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> para configurar los puertos y los prefijos de dirección URL para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fd8e0-806">`UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` y la variable de entorno `ASPNETCORE_URLS` también funcionan, pero tienen las limitaciones que se indican más adelante en esta sección (debe haber disponible un certificado predeterminado para la configuración de puntos de conexión HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fd8e0-807">Configuración de `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fd8e0-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fd8e0-809">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión especificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fd8e0-810">Al llamar a `ConfigureEndpointDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fd8e0-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fd8e0-812">Especifica una `Action` de configuración para que se ejecute con cada punto de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fd8e0-813">Al llamar a `ConfigureHttpsDefaults` varias veces, se reemplazan las `Action` anteriores por la última `Action` especificada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="fd8e0-814">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="fd8e0-815">Crea un cargador de configuración para configurar Kestrel que toma una <xref:Microsoft.Extensions.Configuration.IConfiguration> como entrada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fd8e0-816">El ámbito de la configuración debe corresponderse con la sección de configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fd8e0-817">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="fd8e0-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fd8e0-818">Configure Kestrel para que use HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fd8e0-819">Extensiones de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fd8e0-820">`UseHttps`: configure Kestrel para que use HTTPS con el certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fd8e0-821">Produce una excepción si no hay ningún certificado predeterminado configurado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-821">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fd8e0-822">Parámetros de `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fd8e0-823">`filename` es la ruta de acceso y el nombre de archivo de un archivo de certificado correspondiente al directorio donde están los archivos de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fd8e0-824">`password` es la contraseña necesaria para obtener acceso a los datos del certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fd8e0-825">`configureOptions` es una `Action` para configurar `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fd8e0-826">Devuelve `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fd8e0-827">`storeName` es el almacén de certificados desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fd8e0-828">`subject` es el nombre del sujeto del certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fd8e0-829">`allowInvalid` indica si se deben tener en cuenta los certificados no válidos, como los certificados autofirmados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fd8e0-830">`location` es la ubicación del almacén desde el que se carga el certificado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fd8e0-831">`serverCertificate` es el certificado X.509.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fd8e0-832">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fd8e0-833">Como mínimo, debe existir un certificado predeterminado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fd8e0-834">Estas son las configuraciones compatibles:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-834">Supported configurations described next:</span></span>

* <span data-ttu-id="fd8e0-835">Sin configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-835">No configuration</span></span>
* <span data-ttu-id="fd8e0-836">Reemplazar el certificado predeterminado de configuración</span><span class="sxs-lookup"><span data-stu-id="fd8e0-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fd8e0-837">Cambiar los valores predeterminados en el código</span><span class="sxs-lookup"><span data-stu-id="fd8e0-837">Change the defaults in code</span></span>

<span data-ttu-id="fd8e0-838">*Sin configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-838">*No configuration*</span></span>

<span data-ttu-id="fd8e0-839">Kestrel escucha en `http://localhost:5000` y en `https://localhost:5001` (si hay disponible un certificado predeterminado).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fd8e0-840">*Reemplazar el certificado predeterminado de configuración*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fd8e0-841">`CreateDefaultBuilder` llama a `Configure(context.Configuration.GetSection("Kestrel"))` de forma predeterminada para cargar la configuración de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fd8e0-842">Hay disponible un esquema de configuración de aplicación HTTPS predeterminado para Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fd8e0-843">Configure varios puntos de conexión (incluidas las direcciones URL y los certificados que va a usar) desde un archivo en disco o desde un almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fd8e0-844">En el siguiente ejemplo de *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fd8e0-845">Establezca **AllowInvalid** en `true` para permitir el uso de certificados no válidos (por ejemplo, certificados autofirmados).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fd8e0-846">Cualquier punto de conexión HTTPS que no especifique un certificado (**HttpsDefaultCert** en el siguiente ejemplo) revierte al certificado definido en **Certificados** > **Predeterminado** o al certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="fd8e0-847">Una alternativa al uso de **Path** y **Password** en cualquier nodo de certificado consiste en especificar el certificado por medio de campos del almacén de certificados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fd8e0-848">Por ejemplo, el certificado en **Certificados** > **Predeterminado** se puede especificar así:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fd8e0-849">Notas sobre el esquema:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-849">Schema notes:</span></span>

* <span data-ttu-id="fd8e0-850">En los nombres de los puntos de conexión se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fd8e0-851">Por ejemplo, `HTTPS` y `Https` son válidos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fd8e0-852">El parámetro `Url` es necesario en cada punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fd8e0-853">El formato de este parámetro es el mismo que el del parámetro de configuración `Urls` de nivel superior, excepto por el hecho de que está limitado a un único valor.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fd8e0-854">En vez de agregarse, estos puntos de conexión reemplazan a los que están definidos en la configuración `Urls` de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fd8e0-855">Los puntos de conexión definidos en el código a través de `Listen` son acumulativos con respecto a los puntos de conexión definidos en la sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fd8e0-856">La sección `Certificate` es opcional.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="fd8e0-857">Si la sección `Certificate` no se especifica, se usan los valores predeterminados definidos en escenarios anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fd8e0-858">Si no hay valores predeterminados disponibles, el servidor produce una excepción y no se inicia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fd8e0-859">La sección `Certificate` admite certificados tanto **Path**&ndash;**Password** como **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fd8e0-860">Se puede definir el número de puntos de conexión que se quiera de esta manera, siempre y cuando no produzcan conflictos de puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fd8e0-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` devuelve un `KestrelConfigurationLoader` con un método `.Endpoint(string name, listenOptions => { })` que se puede usar para complementar la configuración de un punto de conexión configurado:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="fd8e0-862">Se puede acceder directamente a `KestrelServerOptions.ConfigurationLoader` para seguir con la iteración en el cargador existente, como el proporcionado por <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fd8e0-863">La sección de configuración de cada punto de conexión está disponible en las opciones del método `Endpoint` para que se pueda leer la configuración personalizada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fd8e0-864">Se pueden cargar varias configuraciones volviendo a llamar a `options.Configure(context.Configuration.GetSection("{SECTION}"))` con otra sección.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fd8e0-865">Se usa la última configuración, a menos que se llame explícitamente a `Load` en instancias anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fd8e0-866">El metapaquete no llama a `Load`, con lo cual su sección de configuración predeterminada se puede reemplazar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fd8e0-867">`KestrelConfigurationLoader` refleja la familia `Listen` de API de `KestrelServerOptions` como sobrecargas de `Endpoint`, por lo que los puntos de conexión de configuración y código se pueden configurar en el mismo lugar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fd8e0-868">En estas sobrecargas no se usan nombres y solo consumen valores predeterminados de la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fd8e0-869">*Cambiar los valores predeterminados en el código*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-869">*Change the defaults in code*</span></span>

<span data-ttu-id="fd8e0-870">`ConfigureEndpointDefaults` y `ConfigureHttpsDefaults` se pueden usar para cambiar la configuración predeterminada de `ListenOptions` y `HttpsConnectionAdapterOptions`, incluido sustituir el certificado predeterminado especificado en el escenario anterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fd8e0-871">Se debe llamar a `ConfigureEndpointDefaults` y a `ConfigureHttpsDefaults` antes de que se configure algún punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="fd8e0-872">*Compatibilidad de Kestrel con SNI*</span><span class="sxs-lookup"><span data-stu-id="fd8e0-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fd8e0-873">[Indicación de nombre de servidor (SNI)](https://tools.ietf.org/html/rfc6066#section-3) se puede usar para hospedar varios dominios en la misma dirección IP y puerto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fd8e0-874">Para que SNI funcione, el cliente envía el nombre de host de la sesión segura al servidor durante el protocolo de enlace TLS para que, de este modo, el servidor pueda proporcionar el certificado correcto.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fd8e0-875">El cliente usa el certificado proporcionado para la comunicación cifrada con el servidor durante la sesión segura que sigue al protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fd8e0-876">Kestrel admite SNI a través de la devolución de llamada `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fd8e0-877">La devolución de llamada se invoca una vez por conexión para permitir que la aplicación inspeccione el nombre de host y seleccione el certificado adecuado.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fd8e0-878">La compatibilidad con SNI requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-878">SNI support requires:</span></span>

* <span data-ttu-id="fd8e0-879">Ejecutarse en el marco de destino `netcoreapp2.1` o posterior.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fd8e0-880">En `net461` o posterior, se invoca la devolución de llamada, pero `name` siempre es `null`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fd8e0-881">`name` también será `null` si el cliente no proporciona el parámetro de nombre de host en el protocolo de enlace TLS.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fd8e0-882">Todos los sitios web deben ejecutarse en la misma instancia de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fd8e0-883">Kestrel no admite el uso compartido de una dirección IP y un puerto entre varias instancias sin un proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fd8e0-884">Enlazar a un socket TCP</span><span class="sxs-lookup"><span data-stu-id="fd8e0-884">Bind to a TCP socket</span></span>

<span data-ttu-id="fd8e0-885">El método <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> se enlaza a un socket TCP y una expresión lambda de opciones permite configurar un certificado X.509:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="fd8e0-886">En el ejemplo se configura HTTPS para un punto de conexión con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fd8e0-887">Use la misma API para configurar otras opciones de Kestrel para puntos de conexión específicos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fd8e0-888">Enlazar a un socket de Unix</span><span class="sxs-lookup"><span data-stu-id="fd8e0-888">Bind to a Unix socket</span></span>

<span data-ttu-id="fd8e0-889">Escuche en un socket de Unix con <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="fd8e0-890">Puerto 0</span><span class="sxs-lookup"><span data-stu-id="fd8e0-890">Port 0</span></span>

<span data-ttu-id="fd8e0-891">Cuando se especifica el número de puerto `0`, Kestrel se enlaza de forma dinámica a un puerto disponible.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fd8e0-892">En el siguiente ejemplo se muestra cómo averiguar qué puerto Kestrel está realmente enlazado a un runtime:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fd8e0-893">Cuando la aplicación se ejecuta, la salida de la ventana de consola indica el puerto dinámico en el que se puede tener acceso a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fd8e0-894">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="fd8e0-894">Limitations</span></span>

<span data-ttu-id="fd8e0-895">Configure puntos de conexión con los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fd8e0-896">El argumento de la línea de comandos `--urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="fd8e0-897">La clave de configuración de host `urls`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-897">`urls` host configuration key</span></span>
* <span data-ttu-id="fd8e0-898">La variable de entorno `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="fd8e0-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fd8e0-899">Estos métodos son útiles para que el código funcione con servidores que no sean de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fd8e0-900">Sin embargo, tenga en cuenta las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fd8e0-901">HTTPS no se puede usar con estos métodos, a menos que se proporcione un certificado predeterminado en la configuración del punto de conexión HTTPS (por ejemplo, por medio de la configuración `KestrelServerOptions` o de un archivo de configuración, tal y como se explicó anteriormente en este tema).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fd8e0-902">Cuando los métodos `Listen` y `UseUrls` se usan al mismo tiempo, los puntos de conexión de `Listen` sustituyen a los de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fd8e0-903">Configuración de puntos de conexión IIS</span><span class="sxs-lookup"><span data-stu-id="fd8e0-903">IIS endpoint configuration</span></span>

<span data-ttu-id="fd8e0-904">Cuando se usa IIS, los enlaces de direcciones URL de IIS reemplazan a los enlaces que se hayan establecido por medio de `Listen` o de `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fd8e0-905">Para más información, vea el tema [Módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fd8e0-906">Configuración de transporte</span><span class="sxs-lookup"><span data-stu-id="fd8e0-906">Transport configuration</span></span>

<span data-ttu-id="fd8e0-907">Desde el lanzamiento de ASP.NET Core 2.1, el transporte predeterminado de Kestrel deja de basarse en Libuv y pasa a basarse en sockets administrados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="fd8e0-908">Se trata de un cambio muy importante para las aplicaciones ASP.NET Core 2.0 que actualizan a 2.1 y llaman a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>, y que dependen de cualquiera de los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="fd8e0-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (referencia de paquete directa)</span><span class="sxs-lookup"><span data-stu-id="fd8e0-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="fd8e0-910">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="fd8e0-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="fd8e0-911">Para los proyectos que necesitan el uso de Libuv:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="fd8e0-912">Agregar una dependencia del paquete [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="fd8e0-913">Llame a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="fd8e0-914">Prefijos de URL</span><span class="sxs-lookup"><span data-stu-id="fd8e0-914">URL prefixes</span></span>

<span data-ttu-id="fd8e0-915">Al usar `UseUrls`, el argumento de línea de comandos `--urls`, la clave de configuración de host `urls` o una variable de entorno `ASPNETCORE_URLS`, los prefijos de dirección URL pueden tener cualquiera de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fd8e0-916">Solo son válidos los prefijos de dirección URL HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fd8e0-917">Kestrel no admite HTTPS al configurar enlaces de dirección URL con `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fd8e0-918">Dirección IPv4 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fd8e0-919">`0.0.0.0` es un caso especial que enlaza a todas las direcciones IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fd8e0-920">Dirección IPv6 con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fd8e0-921">`[::]` es el equivalente en IPv6 de `0.0.0.0` en IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fd8e0-922">Nombre de host con número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fd8e0-923">Los nombres de host, `*` y `+` no son especiales.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fd8e0-924">Todo lo que no se identifique como una dirección IP o un `localhost` válido se enlaza a todas las direcciones IP de IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fd8e0-925">Para enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](xref:fundamentals/servers/httpsys) o un servidor proxy inverso, como IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fd8e0-926">El hospedaje en una configuración de proxy inverso requiere [filtrado de hosts](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fd8e0-927">Nombre `localhost` del host con el número de puerto o la IP de bucle invertido con el número de puerto</span><span class="sxs-lookup"><span data-stu-id="fd8e0-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fd8e0-928">Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fd8e0-929">Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fd8e0-930">Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fd8e0-931">Filtrado de hosts</span><span class="sxs-lookup"><span data-stu-id="fd8e0-931">Host filtering</span></span>

<span data-ttu-id="fd8e0-932">Si bien Kestrel admite una configuración basada en prefijos como `http://example.com:5000`, pasa por alto completamente el nombre de host.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fd8e0-933">El host `localhost` es un caso especial que se usa para enlazar a direcciones de bucle invertido.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fd8e0-934">Cualquier otro host que no sea una dirección IP explícita se enlaza a todas las direcciones IP públicas.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fd8e0-935">Los encabezados `Host` no están validados.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fd8e0-936">Como solución alternativa, use el Middleware de filtrado de hosts.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fd8e0-937">El middleware de filtrado de host se facilita a través del paquete [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o 2.2).</span><span class="sxs-lookup"><span data-stu-id="fd8e0-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="fd8e0-938">El middleware se agrega por medio de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, que llama a <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fd8e0-939">El Middleware de filtrado de hosts está deshabilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fd8e0-940">Para habilitarlo, defina una clave `AllowedHosts` en *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fd8e0-941">El valor es una lista delimitada por punto y coma de nombres de host sin los números de puerto:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fd8e0-942">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fd8e0-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fd8e0-943">[Middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) también tiene una opción <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fd8e0-944">El Middleware de encabezados reenviados y el Middleware de filtrado de hosts tienen una funcionalidad similar en diferentes escenarios.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fd8e0-945">Establecer `AllowedHosts` con el Middleware de encabezados reenviados es adecuado cuando el encabezado `Host` no se conserva mientras se reenvían solicitudes con un servidor proxy inverso o un equilibrador de carga.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fd8e0-946">Establecer `AllowedHosts` con el Middleware de filtrado de hosts es adecuado cuando se usa Kestrel como un servidor perimetral de acceso público o cuando el encabezado `Host` se reenvía directamente.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fd8e0-947">Para obtener más información sobre el Middleware de encabezados reenviados, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fd8e0-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fd8e0-948">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fd8e0-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* <span data-ttu-id="fd8e0-949">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4) (RFC 7230: Enrutamiento y sintaxis de mensajes [Sección 5.4: Host])</span><span class="sxs-lookup"><span data-stu-id="fd8e0-949">[RFC 7230: Message Syntax and Routing (Section 5.4: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)</span></span>
