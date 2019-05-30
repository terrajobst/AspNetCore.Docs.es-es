---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Obtenga información sobre los conceptos básicos al escribir servicios gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 190004de8b70a463f9f58a25164d5a86ecc266d6
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376356"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="193ae-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="193ae-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="193ae-104">Este documento muestra cómo empezar a trabajar con servicios gRPC mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="193ae-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="193ae-105">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="193ae-105">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="193ae-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="193ae-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="193ae-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="193ae-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="193ae-108">Consulte [empezar a trabajar con servicios gRPC](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto gRPC.</span><span class="sxs-lookup"><span data-stu-id="193ae-108">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="193ae-109">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="193ae-109">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="193ae-110">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="193ae-110">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="193ae-111">Agregar servicios de gRPC a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="193ae-111">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="193ae-112">gRPC requiere los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="193ae-112">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="193ae-113">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="193ae-113">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="193ae-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) para protobuf las API del mensaje.</span><span class="sxs-lookup"><span data-stu-id="193ae-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="193ae-115">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="193ae-115">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="193ae-116">Configurar gRPC</span><span class="sxs-lookup"><span data-stu-id="193ae-116">Configure gRPC</span></span>

<span data-ttu-id="193ae-117">gRPC está habilitado con el `AddGrpc` método:</span><span class="sxs-lookup"><span data-stu-id="193ae-117">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="193ae-118">Cada servicio gRPC se agrega a la canalización de enrutamiento a través de la `MapGrpcService` método:</span><span class="sxs-lookup"><span data-stu-id="193ae-118">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

<span data-ttu-id="193ae-119">Características y middleware de ASP.NET Core comparten la canalización de enrutamiento, por lo tanto, una aplicación puede configurarse para dar servicio a los controladores de solicitudes adicionales.</span><span class="sxs-lookup"><span data-stu-id="193ae-119">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="193ae-120">Los controladores de solicitud adicionales, como controladores de MVC, trabajaran en paralelo con los servicios configurados gRPC.</span><span class="sxs-lookup"><span data-stu-id="193ae-120">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="193ae-121">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="193ae-121">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="193ae-122">gRPC services tienen acceso completo a las características de ASP.NET Core como [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) y [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="193ae-122">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="193ae-123">Por ejemplo, la implementación del servicio puede resolver un servicio de registrador desde el contenedor de DI a través del constructor:</span><span class="sxs-lookup"><span data-stu-id="193ae-123">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="193ae-124">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de inserción de dependencias con cualquier duración (Singleton, en el ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="193ae-124">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="193ae-125">Resolver HttpContext en gRPC métodos</span><span class="sxs-lookup"><span data-stu-id="193ae-125">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="193ae-126">La API gRPC proporciona acceso a algunos datos de mensaje HTTP/2, como el método, host, encabezado y finalizadores.</span><span class="sxs-lookup"><span data-stu-id="193ae-126">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="193ae-127">Es el acceso a través de la `ServerCallContext` argumento pasado a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="193ae-127">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="193ae-128">`ServerCallContext` no proporciona acceso completo a `HttpContext` en todas las APIs ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="193ae-128">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="193ae-129">El `GetHttpContext` método de extensión proporciona acceso completo a la `HttpContext` que representa el mensaje de HTTP/2 subyacente en ASP.NET APIs:</span><span class="sxs-lookup"><span data-stu-id="193ae-129">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="193ae-130">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="193ae-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
