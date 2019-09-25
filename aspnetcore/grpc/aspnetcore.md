---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 18a6dd2ddd4f3c3c4466e3b96dd1748fd0972e39
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250799"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="d6aa9-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6aa9-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="d6aa9-104">En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6aa9-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d6aa9-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6aa9-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6aa9-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d6aa9-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d6aa9-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d6aa9-108">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d6aa9-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="d6aa9-109">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6aa9-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="d6aa9-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d6aa9-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6aa9-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d6aa9-112">Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="d6aa9-113">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d6aa9-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="d6aa9-114">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="d6aa9-115">Incorporación de gRPC Services a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6aa9-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="d6aa9-116">gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="d6aa9-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="d6aa9-117">Configuración de gRPC</span><span class="sxs-lookup"><span data-stu-id="d6aa9-117">Configure gRPC</span></span>

<span data-ttu-id="d6aa9-118">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="d6aa9-119">gRPC se habilita con el `AddGrpc` método.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="d6aa9-120">Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="d6aa9-121">ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="d6aa9-122">Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="d6aa9-123">Configuración de Kestrel</span><span class="sxs-lookup"><span data-stu-id="d6aa9-123">Configure Kestrel</span></span>

<span data-ttu-id="d6aa9-124">Puntos de conexión de Kestrel gRPC:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="d6aa9-125">Requiere HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-125">Require HTTP/2.</span></span>
* <span data-ttu-id="d6aa9-126">Debe protegerse con la seguridad de la [capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-126">Should be secured with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span>

#### <a name="http2"></a><span data-ttu-id="d6aa9-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="d6aa9-127">HTTP/2</span></span>

<span data-ttu-id="d6aa9-128">gRPC requiere HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="d6aa9-129">gRPC for ASP.NET Core valida [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) es `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="d6aa9-130">Kestrel [admite http/2](xref:fundamentals/servers/kestrel#http2-support) en la mayoría de los sistemas operativos modernos.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="d6aa9-131">Los puntos de conexión de Kestrel se configuran para admitir conexiones HTTP/1.1 y HTTP/2 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="tls"></a><span data-ttu-id="d6aa9-132">TLS</span><span class="sxs-lookup"><span data-stu-id="d6aa9-132">TLS</span></span>

<span data-ttu-id="d6aa9-133">Los puntos de conexión de Kestrel usados para gRPC deben protegerse con TLS.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-133">Kestrel endpoints used for gRPC should be secured with TLS.</span></span> <span data-ttu-id="d6aa9-134">En el desarrollo, se crea `https://localhost:5001` automáticamente un punto de conexión protegido con TLS cuando el certificado de desarrollo de ASP.net Core está presente.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-134">In development, an endpoint secured with TLS is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="d6aa9-135">No se requiere ninguna configuración.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-135">No configuration is required.</span></span> <span data-ttu-id="d6aa9-136">Un `https` prefijo comprueba que el punto de conexión Kestrel usa TLS.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-136">An `https` prefix verifies the Kestrel endpoint is using TLS.</span></span>

<span data-ttu-id="d6aa9-137">En producción, se debe configurar explícitamente TLS.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-137">In production, TLS must be explicitly configured.</span></span> <span data-ttu-id="d6aa9-138">En el siguiente ejemplo de *appSettings. JSON* , se proporciona un punto de conexión http/2 protegido con TLS:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with TLS is provided:</span></span>

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

<span data-ttu-id="d6aa9-139">Como alternativa, se pueden configurar puntos de conexión de Kestrel en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-139">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a><span data-ttu-id="d6aa9-140">Negociación de protocolo</span><span class="sxs-lookup"><span data-stu-id="d6aa9-140">Protocol negotiation</span></span>

<span data-ttu-id="d6aa9-141">TLS se usa para más que proteger la comunicación.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-141">TLS is used for more than securing communication.</span></span> <span data-ttu-id="d6aa9-142">El protocolo [de enlace de negociación del Protocolo de capa de aplicación (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) de TLS se usa para negociar el protocolo de conexión entre el cliente y el servidor cuando un extremo admite varios protocolos.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-142">The TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake is used to negotiate the connection protocol between the client and the server when an endpoint supports multiple protocols.</span></span> <span data-ttu-id="d6aa9-143">Esta negociación determina si la conexión utiliza HTTP/1.1 o HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-143">This negotiation determines whether the connection uses HTTP/1.1 or HTTP/2.</span></span>

<span data-ttu-id="d6aa9-144">Si un punto de conexión HTTP/2 se configura sin TLS, el [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) del punto de `HttpProtocols.Http2`conexión debe establecerse en.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-144">If an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="d6aa9-145">No se puede usar un punto de conexión con `HttpProtocols.Http1AndHttp2`varios protocolos (por ejemplo,) sin TLS porque no hay ninguna negociación.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-145">An endpoint with multiple protocols (for example, `HttpProtocols.Http1AndHttp2`) can't be used without TLS because there is no negotiation.</span></span> <span data-ttu-id="d6aa9-146">Se produce un error en todas las conexiones al extremo no seguro de forma predeterminada a HTTP/1.1 y a las llamadas a gRPC.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-146">All connections to the unsecured endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="d6aa9-147">Para obtener más información sobre cómo habilitar HTTP/2 y TLS con Kestrel, consulte [configuración del punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-147">For more information on enabling HTTP/2 and TLS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="d6aa9-148">macOS no admite gRPC de ASP.NET Core con TLS.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-148">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="d6aa9-149">Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-149">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="d6aa9-150">Para obtener más información, vea [No se puede iniciar la aplicación gRPC de ASP.NET Core en macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-150">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="d6aa9-151">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6aa9-151">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="d6aa9-152">los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción](xref:fundamentals/dependency-injection) de dependencias (di) y el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-152">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d6aa9-153">Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-153">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="d6aa9-154">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="d6aa9-154">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="d6aa9-155">Resolver HttpContext en métodos gRPC</span><span class="sxs-lookup"><span data-stu-id="d6aa9-155">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="d6aa9-156">La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-156">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="d6aa9-157">El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-157">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="d6aa9-158">`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="d6aa9-158">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="d6aa9-159">El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:</span><span class="sxs-lookup"><span data-stu-id="d6aa9-159">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="d6aa9-160">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d6aa9-160">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
