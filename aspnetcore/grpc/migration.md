---
title: Migración de gRPC Services desde C-Core a ASP.NET Core
author: juntaoluo
description: Obtenga información sobre cómo migrar una aplicación de gRPC basada en C-Core existente para que se ejecute en la parte superior de la pila de ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: c4c07808540c9af370bfa253e8154a8a19f0f3de
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634065"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Migración de gRPC Services desde C-Core a ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

Debido a la implementación de la pila subyacente, no todas las características funcionan de la misma manera entre aplicaciones [gRPC basadas en C-Core](https://grpc.io/blog/grpc-stacks) y aplicaciones basadas en ASP.net Core. En este documento se resaltan las diferencias principales para la migración entre las dos pilas.

## <a name="grpc-service-implementation-lifetime"></a>duración de la implementación del servicio gRPC

En la pila de ASP.NET Core, gRPC Services, de forma predeterminada, se crean con un [ámbito de duración](xref:fundamentals/dependency-injection#service-lifetimes). En cambio, gRPC C-Core de forma predeterminada se enlaza a un servicio con una [duración singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Una duración de ámbito permite que la implementación del servicio resuelva otros servicios con duraciones de ámbito. Por ejemplo, una duración de ámbito también puede resolver `DbContext` del contenedor de DI a través de la inserción de constructores. Usar vigencia de ámbito:

* Se crea una nueva instancia de la implementación del servicio para cada solicitud.
* No es posible compartir el estado entre solicitudes a través de miembros de instancia en el tipo de implementación.
* La expectativa es almacenar Estados compartidos en un servicio Singleton en el contenedor de DI. Los Estados compartidos almacenados se resuelven en el constructor de la implementación del servicio gRPC.

Para obtener más información sobre la duración de los servicios, consulte <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Agregar un servicio singleton

Para facilitar la transición de una implementación de gRPC C-Core a ASP.NET Core, es posible cambiar la duración del servicio de la implementación del servicio de ámbito a singleton. Esto implica agregar una instancia de la implementación del servicio al contenedor de DI:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Sin embargo, una implementación de servicio con una duración singleton ya no puede resolver los servicios de ámbito a través de la inserción de constructores.

## <a name="configure-grpc-services-options"></a>Configurar opciones de gRPC Services

En las aplicaciones basadas en C-Core, los valores como `grpc.max_receive_message_length` y `grpc.max_send_message_length` se configuran con `ChannelOption` al [construir la instancia de servidor](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

En ASP.NET Core, gRPC proporciona la configuración a través del tipo de `GrpcServiceOptions`. Por ejemplo, el tamaño máximo del mensaje entrante del servicio gRPC se puede configurar a través de `AddGrpc`. En el ejemplo siguiente se cambia la `MaxReceiveMessageSize` predeterminada de 4 MB a 16 MB:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

Para obtener más información sobre la configuración, vea <xref:grpc/configuration>.

## <a name="logging"></a>Registro

Las aplicaciones basadas en C-Core se basan en el `GrpcEnvironment` para [configurar el registrador](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) con fines de depuración. La pila de ASP.NET Core proporciona esta funcionalidad a través de la [API de registro](xref:fundamentals/logging/index). Por ejemplo, se puede Agregar un registrador al servicio gRPC a través de la inserción de constructores:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Las aplicaciones basadas en C-Core configuran HTTPS mediante la [propiedad Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Se usa un concepto similar para configurar servidores en ASP.NET Core. Por ejemplo, Kestrel usa la [configuración de extremo](xref:fundamentals/servers/kestrel#endpoint-configuration) para esta funcionalidad.

## <a name="grpc-interceptors-vs-middleware"></a>interceptores de gRPC frente a middleware

ASP.NET Core [middleware](xref:fundamentals/middleware/index) ofrece funcionalidades similares en comparación con los interceptores de aplicaciones gRPC basadas en C-Core. ASP.NET Core middleware y los interceptores son conceptualmente similares. Ambos

* Se usan para construir una canalización que controla una solicitud gRPC.
* Permita que el trabajo se realice antes o después del siguiente componente en la canalización.
* Proporcione acceso a `HttpContext`:
  * En middleware, el `HttpContext` es un parámetro.
  * En los interceptores se puede tener acceso al `HttpContext` mediante el parámetro `ServerCallContext` con el método de extensión `ServerCallContext.GetHttpContext`. Tenga en cuenta que esta característica es específica de los interceptores que se ejecutan en ASP.NET Core.

diferencias entre el interceptor de gRPC y el middleware ASP.NET Core:

* Interceptores
  * Opere en el nivel de abstracción gRPC mediante [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).
  * Proporcionar acceso a:
    * El mensaje deserializado que se envía a una llamada.
    * Mensaje que se devuelve desde la llamada antes de que se serialice.
* Middleware
  * Se ejecuta antes que los interceptores de gRPC.
  * Opera en los mensajes HTTP/2 subyacentes.
  * Solo puede obtener acceso a los bytes de las secuencias de solicitud y respuesta.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
