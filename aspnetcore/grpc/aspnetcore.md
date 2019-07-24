---
title: Servicios gRPC con ASP.NET Core
author: juntaoluo
description: Conozca los conceptos básicos al escribir servicios de gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 02e443dfecf2f7464a8ecabfc0cac67854d63232
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412480"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="5b035-103">Servicios gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b035-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="5b035-104">En este documento se muestra cómo empezar a trabajar con gRPC Services mediante ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b035-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b035-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5b035-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5b035-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b035-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5b035-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5b035-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5b035-108">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5b035-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="5b035-109">Introducción al servicio gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b035-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="5b035-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5b035-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5b035-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b035-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5b035-112">Vea Introducción [a gRPC Services](xref:tutorials/grpc/grpc-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de gRPC.</span><span class="sxs-lookup"><span data-stu-id="5b035-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5b035-113">Visual Studio Code/Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="5b035-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="5b035-114">Ejecute `dotnet new grpc -o GrpcGreeter` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5b035-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="5b035-115">Incorporación de gRPC Services a una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b035-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="5b035-116">gRPC requiere el paquete [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="5b035-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="5b035-117">Configuración de gRPC</span><span class="sxs-lookup"><span data-stu-id="5b035-117">Configure gRPC</span></span>

<span data-ttu-id="5b035-118">gRPC se habilita con el `AddGrpc` método:</span><span class="sxs-lookup"><span data-stu-id="5b035-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="5b035-119">Cada servicio gRPC se agrega a la canalización de enrutamiento `MapGrpcService` a través del método:</span><span class="sxs-lookup"><span data-stu-id="5b035-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="5b035-120">ASP.NET Core los middleware y las características comparten la canalización de enrutamiento, por lo que se puede configurar una aplicación para que atienda a los controladores de solicitud adicionales.</span><span class="sxs-lookup"><span data-stu-id="5b035-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="5b035-121">Los controladores de solicitud adicionales, como los controladores MVC, funcionan en paralelo con los servicios gRPC configurados.</span><span class="sxs-lookup"><span data-stu-id="5b035-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="5b035-122">Integración con API de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b035-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="5b035-123">los servicios de gRPC tienen acceso completo a las características de ASP.NET Core, como la [inserción](xref:fundamentals/dependency-injection) de dependencias (di) y el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="5b035-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="5b035-124">Por ejemplo, la implementación del servicio puede resolver un servicio del registrador a partir del contenedor de DI mediante el constructor:</span><span class="sxs-lookup"><span data-stu-id="5b035-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="5b035-125">De forma predeterminada, la implementación del servicio gRPC puede resolver otros servicios de DI con cualquier duración (singleton, ámbito o transitorio).</span><span class="sxs-lookup"><span data-stu-id="5b035-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="5b035-126">Resolver HttpContext en métodos gRPC</span><span class="sxs-lookup"><span data-stu-id="5b035-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="5b035-127">La API gRPC proporciona acceso a algunos datos de mensajes HTTP/2, como el método, el host, el encabezado y los finalizadores.</span><span class="sxs-lookup"><span data-stu-id="5b035-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="5b035-128">El acceso se realiza `ServerCallContext` a través del argumento que se pasa a cada método gRPC:</span><span class="sxs-lookup"><span data-stu-id="5b035-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="5b035-129">`ServerCallContext`no proporciona acceso completo a `HttpContext` en todas las API de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="5b035-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="5b035-130">El `GetHttpContext` método de extensión proporciona acceso completo `HttpContext` a que representa el mensaje http/2 subyacente en las API de ASP.net:</span><span class="sxs-lookup"><span data-stu-id="5b035-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="5b035-131">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5b035-131">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
