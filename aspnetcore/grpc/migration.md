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
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migrar los servicios gRPC de núcleo de C a ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

Debido a la implementación de la pila subyacente, no todas las características funcionan del mismo modo entre [basada en núcleo C gRPC](https://grpc.io/blog/grpc-stacks) aplicaciones y las aplicaciones basadas en ASP.NET Core. Este artículo destacan las diferencias principales para migrar entre las dos pilas.

## <a name="grpc-service-implementation-lifetime"></a>duración de implementación del servicio gRPC

En la pila de ASP.NET Core, servicios de gRPC, de forma predeterminada, se crean con un [duración con ámbito](xref:fundamentals/dependency-injection#service-lifetimes). En cambio, gRPC C-core de forma predeterminada se enlaza a un servicio con un [duración de singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Una duración con ámbito permite la implementación del servicio resolver otros servicios con la duración con ámbito. Por ejemplo, también puede resolver una duración con ámbito `DBContext` desde el contenedor de DI a través de la inserción del constructor. Uso de duración con ámbito:

* Se construye una nueva instancia de la implementación del servicio para cada solicitud.
* No se puede compartir el estado entre las solicitudes a través de los miembros de instancia en el tipo de implementación.
* La expectativa es almacenar los estados compartidos en un servicio de singleton en el contenedor de DI. Se resuelven los estados compartidos almacenados en el constructor de la implementación del servicio gRPC.

Para obtener más información sobre la duración de los servicios, consulte <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Agregar un servicio de singleton

Para facilitar la transición de una implementación de C-core gRPC a ASP.NET Core, es posible cambiar la duración del servicio de la implementación del servicio de ámbito de singleton. Esto implica la adición de una instancia de la implementación del servicio para el contenedor de DI:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Sin embargo, una implementación de servicio con una duración de singleton ya no es capaz de resolver los servicios con ámbito a través de la inserción del constructor.

## <a name="configure-grpc-services-options"></a>Configurar las opciones de servicios gRPC

En aplicaciones basadas en C-core valores como `grpc.max_receive_message_length` y `grpc.max_send_message_length` se configuran con `ChannelOption` cuando [construir la instancia del servidor](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

En ASP.NET Core, gRPC proporciona la configuración a través de la `GrpcServiceOptions` tipo. Por ejemplo, gRPC de un servicio se puede configurar el tamaño máximo de mensaje entrante a través de `AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

Para obtener más información sobre la configuración, consulte <xref:grpc/configuration>.

## <a name="logging"></a>Registro

Las aplicaciones basada en núcleo C dependen de la `GrpcEnvironment` a [configurar el registrador](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) con fines de depuración. La pila de ASP.NET Core proporciona esta funcionalidad a través de la [API de registro](xref:fundamentals/logging/index). Por ejemplo, un registrador puede agregarse al servicio gRPC mediante la inserción de constructor:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Configuración de aplicaciones basada en núcleo C HTTPS a través de la [Server.Ports propiedad](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Un concepto similar se utiliza para configurar servidores de ASP.NET Core. Por ejemplo, se usa Kestrel [configuración de extremo](xref:fundamentals/servers/kestrel#endpoint-configuration) para esta funcionalidad.

## <a name="interceptors-and-middleware"></a>Los interceptores y Middleware

ASP.NET Core [middleware](xref:fundamentals/middleware/index) ofrece funcionalidades similares en comparación con los interceptores de aplicaciones basada en núcleo C gRPC. Middleware y los interceptores son conceptualmente iguales ya que ambos se usan para construir una canalización que controla una solicitud gRPC. Ambos permiten que se realice antes o después del siguiente componente de la canalización de trabajo. Sin embargo, middleware de ASP.NET Core funciona en los mensajes subyacentes de HTTP/2, mientras que los interceptores de operan en la capa gRPC del uso de abstracción del [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
