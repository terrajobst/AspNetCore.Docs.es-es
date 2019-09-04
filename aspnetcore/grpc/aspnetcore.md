---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 28e6b8589bbe0b6a3723b64736c723c883302571
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238166"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="c4a9f-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a9f-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="c4a9f-104">En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4a9f-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c4a9f-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a9f-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a9f-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c4a9f-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4a9f-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c4a9f-108">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4a9f-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="c4a9f-109">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a9f-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="c4a9f-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a9f-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a9f-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c4a9f-112">Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c4a9f-113">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c4a9f-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c4a9f-114">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="c4a9f-115">Incorporación de gRPC Services a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a9f-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="c4a9f-116">gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="c4a9f-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="c4a9f-117">Configuración de gRPC</span><span class="sxs-lookup"><span data-stu-id="c4a9f-117">Configure gRPC</span></span>

<span data-ttu-id="c4a9f-118">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="c4a9f-119">gRPC se habilita con el `AddGrpc` método.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="c4a9f-120">Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="c4a9f-121">ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="c4a9f-122">Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="c4a9f-123">Configuración de Kestrel</span><span class="sxs-lookup"><span data-stu-id="c4a9f-123">Configure Kestrel</span></span>

<span data-ttu-id="c4a9f-124">Puntos de conexión de Kestrel gRPC:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="c4a9f-125">Requiere HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-125">Require HTTP/2.</span></span>
* <span data-ttu-id="c4a9f-126">Debe protegerse con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="c4a9f-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c4a9f-127">HTTP/2</span></span>

<span data-ttu-id="c4a9f-128">gRPC requiere HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="c4a9f-129">gRPC for ASP.NET Core valida [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) es `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="c4a9f-130">Kestrel [admite http/2](xref:fundamentals/servers/kestrel#http2-support) en la mayoría de los sistemas operativos modernos.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="c4a9f-131">Los puntos de conexión de Kestrel se configuran para admitir conexiones HTTP/1.1 y HTTP/2 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="https"></a><span data-ttu-id="c4a9f-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c4a9f-132">HTTPS</span></span>

<span data-ttu-id="c4a9f-133">Los puntos de conexión de Kestrel usados para gRPC deben estar protegidos con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-133">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="c4a9f-134">En el desarrollo, se crea `https://localhost:5001` automáticamente un punto de conexión HTTPS cuando el certificado de desarrollo de ASP.net Core está presente.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-134">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="c4a9f-135">No se requiere ninguna configuración.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-135">No configuration is required.</span></span>

<span data-ttu-id="c4a9f-136">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-136">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="c4a9f-137">En el siguiente ejemplo de *appSettings. JSON* , se proporciona un punto de conexión http/2 protegido con https:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-137">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
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

<span data-ttu-id="c4a9f-138">Como alternativa, se pueden configurar puntos de conexión de Kestrel en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-138">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="c4a9f-139">Cuando se configura un punto de conexión HTTP/2 sin HTTPS, los [protocolos ListenOptions. Protocol](xref:fundamentals/servers/kestrel#listenoptionsprotocols) del punto de `HttpProtocols.Http2`conexión deben establecerse en.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-139">When an HTTP/2 endpoint is configured without HTTPS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="c4a9f-140">`HttpProtocols.Http1AndHttp2`no se puede usar porque se requiere HTTPS para negociar HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-140">`HttpProtocols.Http1AndHttp2` can't be used because HTTPS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="c4a9f-141">Sin HTTPS, se produce un error en todas las conexiones con el punto de conexión de forma predeterminada a HTTP/1.1 y las llamadas a gRPC.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-141">Without HTTPS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="c4a9f-142">Para obtener más información sobre cómo habilitar HTTP/2 y HTTPS con Kestrel, consulte [configuración del punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-142">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="c4a9f-143">macOS no admite ASP.NET Core gRPC con [seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-143">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="c4a9f-144">Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-144">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="c4a9f-145">Para obtener más información, vea [No se puede iniciar la aplicación gRPC de ASP.NET Core en macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-145">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="c4a9f-146">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4a9f-146">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="c4a9f-147">los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción de dependencias](xref:fundamentals/dependency-injection) (di) y el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-147">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="c4a9f-148">Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-148">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="c4a9f-149">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="c4a9f-149">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="c4a9f-150">Resolver HttpContext en métodos gRPC</span><span class="sxs-lookup"><span data-stu-id="c4a9f-150">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="c4a9f-151">La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-151">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="c4a9f-152">El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-152">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="c4a9f-153">`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="c4a9f-153">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="c4a9f-154">El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:</span><span class="sxs-lookup"><span data-stu-id="c4a9f-154">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="c4a9f-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c4a9f-155">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
