---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130701"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="44cb4-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cb4-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="44cb4-104">En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="44cb4-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44cb4-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="44cb4-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="44cb4-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44cb4-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="44cb4-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44cb4-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="44cb4-108">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="44cb4-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="44cb4-109">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cb4-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="44cb4-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="44cb4-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="44cb4-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44cb4-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="44cb4-112">Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.</span><span class="sxs-lookup"><span data-stu-id="44cb4-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="44cb4-113">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="44cb4-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="44cb4-114">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="44cb4-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="44cb4-115">Incorporación de gRPC Services a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cb4-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="44cb4-116">gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="44cb4-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="44cb4-117">Configuración de gRPC</span><span class="sxs-lookup"><span data-stu-id="44cb4-117">Configure gRPC</span></span>

<span data-ttu-id="44cb4-118">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="44cb4-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="44cb4-119">gRPC se habilita con el `AddGrpc` método.</span><span class="sxs-lookup"><span data-stu-id="44cb4-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="44cb4-120">Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método.</span><span class="sxs-lookup"><span data-stu-id="44cb4-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="44cb4-121">ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales.</span><span class="sxs-lookup"><span data-stu-id="44cb4-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="44cb4-122">Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.</span><span class="sxs-lookup"><span data-stu-id="44cb4-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="44cb4-123">Configuración de Kestrel</span><span class="sxs-lookup"><span data-stu-id="44cb4-123">Configure Kestrel</span></span>

<span data-ttu-id="44cb4-124">Puntos de conexión de Kestrel gRPC:</span><span class="sxs-lookup"><span data-stu-id="44cb4-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="44cb4-125">Requiere HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="44cb4-125">Require HTTP/2.</span></span>
* <span data-ttu-id="44cb4-126">Debe protegerse con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="44cb4-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="44cb4-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="44cb4-127">HTTP/2</span></span>

<span data-ttu-id="44cb4-128">Kestrel [admite http/2](xref:fundamentals/servers/kestrel#http2-support) en la mayoría de los sistemas operativos modernos.</span><span class="sxs-lookup"><span data-stu-id="44cb4-128">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="44cb4-129">Los puntos de conexión de Kestrel se configuran para admitir conexiones HTTP/1.1 y HTTP/2 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="44cb4-129">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

> [!NOTE]
> <span data-ttu-id="44cb4-130">macOS no admite ASP.NET Core gRPC con [seguridad de la capa de transporte (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="44cb4-130">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="44cb4-131">Se requiere configuración adicional para ejecutar correctamente servicios gRPC en macOS.</span><span class="sxs-lookup"><span data-stu-id="44cb4-131">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="44cb4-132">Para obtener más información, vea [No se puede iniciar la aplicación gRPC de ASP.NET Core en macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="44cb4-132">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

#### <a name="https"></a><span data-ttu-id="44cb4-133">HTTPS</span><span class="sxs-lookup"><span data-stu-id="44cb4-133">HTTPS</span></span>

<span data-ttu-id="44cb4-134">Los puntos de conexión de Kestrel usados para gRPC deben estar protegidos con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="44cb4-134">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="44cb4-135">En el desarrollo, se crea `https://localhost:5001` automáticamente un punto de conexión HTTPS cuando el certificado de desarrollo de ASP.net Core está presente.</span><span class="sxs-lookup"><span data-stu-id="44cb4-135">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="44cb4-136">No se requiere ninguna configuración.</span><span class="sxs-lookup"><span data-stu-id="44cb4-136">No configuration is required.</span></span>

<span data-ttu-id="44cb4-137">En un entorno de producción, HTTPS se debe configurar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="44cb4-137">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="44cb4-138">En el siguiente ejemplo de *appSettings. JSON* , se proporciona un punto de conexión http/2 protegido con https:</span><span class="sxs-lookup"><span data-stu-id="44cb4-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

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

<span data-ttu-id="44cb4-139">También se puede configurar Kestrel endspoints en *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="44cb4-139">Alternatively, Kestrel endspoints can be configured in *Program.cs*:</span></span>

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

<span data-ttu-id="44cb4-140">Para obtener más información sobre cómo habilitar HTTP/2 y HTTPS con Kestrel, consulte [configuración del punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="44cb4-140">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="44cb4-141">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44cb4-141">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="44cb4-142">los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción](xref:fundamentals/dependency-injection) de dependencias (di) y el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="44cb4-142">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="44cb4-143">Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:</span><span class="sxs-lookup"><span data-stu-id="44cb4-143">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="44cb4-144">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="44cb4-144">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="44cb4-145">Resolver HttpContext en métodos gRPC</span><span class="sxs-lookup"><span data-stu-id="44cb4-145">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="44cb4-146">La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores.</span><span class="sxs-lookup"><span data-stu-id="44cb4-146">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="44cb4-147">El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="44cb4-147">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="44cb4-148">`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="44cb4-148">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="44cb4-149">El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:</span><span class="sxs-lookup"><span data-stu-id="44cb4-149">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="44cb4-150">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="44cb4-150">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
