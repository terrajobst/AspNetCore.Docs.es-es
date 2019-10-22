---
title: Migración de gRPC Services desde C-Core a ASP.NET Core
author: juntaoluo
description: Obtenga información sobre cómo migrar una aplicación de gRPC basada en C-Core existente para que se ejecute en la parte superior de la pila de ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 596eca0f510387a18472eb353672980e0a8e0d24
ms.sourcegitcommit: eb4fcdeb2f9e8413117624de42841a4997d1d82d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72698001"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="14dca-103">Migración de gRPC Services desde C-Core a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14dca-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="14dca-104">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="14dca-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="14dca-105">Debido a la implementación de la pila subyacente, no todas las características funcionan de la misma manera entre aplicaciones [gRPC basadas en C-Core](https://grpc.io/blog/grpc-stacks) y aplicaciones basadas en ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="14dca-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="14dca-106">En este documento se resaltan las diferencias principales para la migración entre las dos pilas.</span><span class="sxs-lookup"><span data-stu-id="14dca-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="14dca-107">duración de la implementación del servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="14dca-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="14dca-108">En la pila de ASP.NET Core, gRPC Services, de forma predeterminada, se crean con un [ámbito de duración](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="14dca-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="14dca-109">En cambio, gRPC C-Core de forma predeterminada se enlaza a un servicio con una [duración singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="14dca-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="14dca-110">Una duración de ámbito permite que la implementación del servicio resuelva otros servicios con duraciones de ámbito.</span><span class="sxs-lookup"><span data-stu-id="14dca-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="14dca-111">Por ejemplo, una duración de ámbito también puede resolver `DbContext` del contenedor de DI a través de la inserción de constructores.</span><span class="sxs-lookup"><span data-stu-id="14dca-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="14dca-112">Usar vigencia de ámbito:</span><span class="sxs-lookup"><span data-stu-id="14dca-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="14dca-113">Se crea una nueva instancia de la implementación del servicio para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="14dca-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="14dca-114">No es posible compartir el estado entre solicitudes a través de miembros de instancia en el tipo de implementación.</span><span class="sxs-lookup"><span data-stu-id="14dca-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="14dca-115">La expectativa es almacenar Estados compartidos en un servicio Singleton en el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="14dca-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="14dca-116">Los Estados compartidos almacenados se resuelven en el constructor de la implementación del servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="14dca-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="14dca-117">Para obtener más información sobre la duración de los servicios, consulte <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="14dca-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="14dca-118">Agregar un servicio singleton</span><span class="sxs-lookup"><span data-stu-id="14dca-118">Add a singleton service</span></span>

<span data-ttu-id="14dca-119">Para facilitar la transición de una implementación de gRPC C-Core a ASP.NET Core, es posible cambiar la duración del servicio de la implementación del servicio de ámbito a singleton.</span><span class="sxs-lookup"><span data-stu-id="14dca-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="14dca-120">Esto implica agregar una instancia de la implementación del servicio al contenedor de DI:</span><span class="sxs-lookup"><span data-stu-id="14dca-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="14dca-121">Sin embargo, una implementación de servicio con una duración singleton ya no puede resolver los servicios de ámbito a través de la inserción de constructores.</span><span class="sxs-lookup"><span data-stu-id="14dca-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="14dca-122">Configurar opciones de gRPC Services</span><span class="sxs-lookup"><span data-stu-id="14dca-122">Configure gRPC services options</span></span>

<span data-ttu-id="14dca-123">En las aplicaciones basadas en C-Core, los valores como `grpc.max_receive_message_length` y `grpc.max_send_message_length` se configuran con `ChannelOption` al [construir la instancia de servidor](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="14dca-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="14dca-124">En ASP.NET Core, gRPC proporciona la configuración a través del tipo de `GrpcServiceOptions`.</span><span class="sxs-lookup"><span data-stu-id="14dca-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="14dca-125">Por ejemplo, el tamaño máximo del mensaje entrante del servicio gRPC se puede configurar a través de `AddGrpc`.</span><span class="sxs-lookup"><span data-stu-id="14dca-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="14dca-126">En el ejemplo siguiente se cambia la `MaxReceiveMessageSize` predeterminada de 4 MB a 16 MB:</span><span class="sxs-lookup"><span data-stu-id="14dca-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="14dca-127">Para obtener más información sobre la configuración, vea <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="14dca-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="14dca-128">Registro</span><span class="sxs-lookup"><span data-stu-id="14dca-128">Logging</span></span>

<span data-ttu-id="14dca-129">Las aplicaciones basadas en C-Core se basan en el `GrpcEnvironment` para [configurar el registrador](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) con fines de depuración.</span><span class="sxs-lookup"><span data-stu-id="14dca-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="14dca-130">La pila de ASP.NET Core proporciona esta funcionalidad a través de la [API de registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="14dca-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="14dca-131">Por ejemplo, se puede Agregar un registrador al servicio gRPC a través de la inserción de constructores:</span><span class="sxs-lookup"><span data-stu-id="14dca-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="14dca-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="14dca-132">HTTPS</span></span>

<span data-ttu-id="14dca-133">Las aplicaciones basadas en C-Core configuran HTTPS mediante la [propiedad Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="14dca-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="14dca-134">Se usa un concepto similar para configurar servidores en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14dca-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="14dca-135">Por ejemplo, Kestrel usa la [configuración de extremo](xref:fundamentals/servers/kestrel#endpoint-configuration) para esta funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="14dca-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="14dca-136">Interceptores y middleware</span><span class="sxs-lookup"><span data-stu-id="14dca-136">Interceptors and Middleware</span></span>

<span data-ttu-id="14dca-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) ofrece funcionalidades similares en comparación con los interceptores de aplicaciones gRPC basadas en C-Core.</span><span class="sxs-lookup"><span data-stu-id="14dca-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="14dca-138">El middleware y los interceptores son conceptualmente los mismos que se usan para construir una canalización que controla una solicitud gRPC.</span><span class="sxs-lookup"><span data-stu-id="14dca-138">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="14dca-139">Ambos permiten que el trabajo se realice antes o después del siguiente componente en la canalización.</span><span class="sxs-lookup"><span data-stu-id="14dca-139">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="14dca-140">Sin embargo, ASP.NET Core middleware funciona en los mensajes HTTP/2 subyacentes, mientras que los interceptores operan en el nivel de abstracción gRPC mediante [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="14dca-140">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14dca-141">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="14dca-141">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
