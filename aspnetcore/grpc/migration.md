---
title: Migrar los servicios gRPC de núcleo de C a ASP.NET Core
author: juntaoluo
description: Obtenga información sobre cómo mover una aplicación existente de gRPC en función de C-core para ejecutarse sobre la pila de ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982606"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="8ba00-103">Migrar los servicios gRPC de núcleo de C a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ba00-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="8ba00-104">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="8ba00-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="8ba00-105">Debido a la implementación de la pila subyacente, no todas las características funcionan del mismo modo entre [basada en núcleo C gRPC](https://grpc.io/blog/grpc-stacks) aplicaciones y las aplicaciones basadas en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ba00-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="8ba00-106">Este artículo destacan las diferencias principales para migrar entre las dos pilas.</span><span class="sxs-lookup"><span data-stu-id="8ba00-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="8ba00-107">duración de implementación del servicio gRPC</span><span class="sxs-lookup"><span data-stu-id="8ba00-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="8ba00-108">En la pila de ASP.NET Core, servicios de gRPC, de forma predeterminada, se crean con un [duración con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="8ba00-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8ba00-109">En cambio, gRPC C-core de forma predeterminada se enlaza a un servicio con un [duración de singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="8ba00-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="8ba00-110">Una duración con ámbito permite la implementación del servicio resolver otros servicios con la duración con ámbito.</span><span class="sxs-lookup"><span data-stu-id="8ba00-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="8ba00-111">Por ejemplo, también puede resolver una duración con ámbito `DBContext` desde el contenedor de DI a través de la inserción del constructor.</span><span class="sxs-lookup"><span data-stu-id="8ba00-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="8ba00-112">Uso de duración con ámbito:</span><span class="sxs-lookup"><span data-stu-id="8ba00-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="8ba00-113">Se construye una nueva instancia de la implementación del servicio para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="8ba00-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="8ba00-114">No se puede compartir el estado entre las solicitudes a través de los miembros de instancia en el tipo de implementación.</span><span class="sxs-lookup"><span data-stu-id="8ba00-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="8ba00-115">La expectativa es almacenar los estados compartidos en un servicio de singleton en el contenedor de DI.</span><span class="sxs-lookup"><span data-stu-id="8ba00-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="8ba00-116">Se resuelven los estados compartidos almacenados en el constructor de la implementación del servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="8ba00-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="8ba00-117">Para obtener más información sobre la duración de los servicios, consulte <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="8ba00-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="8ba00-118">Agregar un servicio de singleton</span><span class="sxs-lookup"><span data-stu-id="8ba00-118">Add a singleton service</span></span>

<span data-ttu-id="8ba00-119">Para facilitar la transición de una implementación de C-core gRPC a ASP.NET Core, es posible cambiar la duración del servicio de la implementación del servicio de ámbito de singleton.</span><span class="sxs-lookup"><span data-stu-id="8ba00-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="8ba00-120">Esto implica la adición de una instancia de la implementación del servicio para el contenedor de DI:</span><span class="sxs-lookup"><span data-stu-id="8ba00-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="8ba00-121">Sin embargo, una implementación de servicio con una duración de singleton ya no es capaz de resolver los servicios con ámbito a través de la inserción del constructor.</span><span class="sxs-lookup"><span data-stu-id="8ba00-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="8ba00-122">Configurar las opciones de servicios gRPC</span><span class="sxs-lookup"><span data-stu-id="8ba00-122">Configure gRPC services options</span></span>

<span data-ttu-id="8ba00-123">En aplicaciones basadas en C-core valores como `grpc.max_receive_message_length` y `grpc.max_send_message_length` se configuran con `ChannelOption` cuando [construir la instancia del servidor](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="8ba00-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="8ba00-124">En ASP.NET Core, gRPC proporciona la configuración a través de la `GrpcServiceOptions` tipo.</span><span class="sxs-lookup"><span data-stu-id="8ba00-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="8ba00-125">Por ejemplo, gRPC de un servicio se puede configurar el tamaño máximo de mensaje entrante a través de `AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="8ba00-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="8ba00-126">Para obtener más información sobre la configuración, consulte <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="8ba00-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="8ba00-127">Registro</span><span class="sxs-lookup"><span data-stu-id="8ba00-127">Logging</span></span>

<span data-ttu-id="8ba00-128">Las aplicaciones basada en núcleo C dependen de la `GrpcEnvironment` a [configurar el registrador](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) con fines de depuración.</span><span class="sxs-lookup"><span data-stu-id="8ba00-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="8ba00-129">La pila de ASP.NET Core proporciona esta funcionalidad a través de la [API de registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="8ba00-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="8ba00-130">Por ejemplo, un registrador puede agregarse al servicio gRPC mediante la inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="8ba00-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="8ba00-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="8ba00-131">HTTPS</span></span>

<span data-ttu-id="8ba00-132">Configuración de aplicaciones basada en núcleo C HTTPS a través de la [Server.Ports propiedad](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="8ba00-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="8ba00-133">Un concepto similar se utiliza para configurar servidores de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ba00-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="8ba00-134">Por ejemplo, se usa Kestrel [configuración de extremo](xref:fundamentals/servers/kestrel#endpoint-configuration) para esta funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="8ba00-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="8ba00-135">Los interceptores y Middleware</span><span class="sxs-lookup"><span data-stu-id="8ba00-135">Interceptors and Middleware</span></span>

<span data-ttu-id="8ba00-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) ofrece funcionalidades similares en comparación con los interceptores de aplicaciones basada en núcleo C gRPC.</span><span class="sxs-lookup"><span data-stu-id="8ba00-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="8ba00-137">Middleware y los interceptores son conceptualmente iguales ya que ambos se usan para construir una canalización que controla una solicitud gRPC.</span><span class="sxs-lookup"><span data-stu-id="8ba00-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="8ba00-138">Ambos permiten que se realice antes o después del siguiente componente de la canalización de trabajo.</span><span class="sxs-lookup"><span data-stu-id="8ba00-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="8ba00-139">Sin embargo, middleware de ASP.NET Core funciona en los mensajes subyacentes de HTTP/2, mientras que los interceptores de operan en la capa gRPC del uso de abstracción del [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="8ba00-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ba00-140">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8ba00-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
