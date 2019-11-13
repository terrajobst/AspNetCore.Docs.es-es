---
title: integración de gRPC Client Factory en .NET Core
author: jamesnk
description: Aprenda a crear clientes de gRPC mediante el generador de cliente.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963681"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>integración de gRPC Client Factory en .NET Core

la integración de gRPC con `HttpClientFactory` ofrece una manera centralizada de crear clientes de gRPC. Se puede usar como alternativa a [la configuración de instancias de cliente de gRPC independientes](xref:grpc/client). La integración de fábrica está disponible en el paquete NuGet [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .

El generador ofrece las siguientes ventajas:

* Proporciona una ubicación central para configurar las instancias de cliente gRPC lógicas
* Administra la duración del `HttpClientMessageHandler` subyacente
* Propagación automática de fecha límite y cancelación en un servicio de gRPC de ASP.NET Core

## <a name="register-grpc-clients"></a>Registro de clientes de gRPC

Para registrar un cliente de gRPC, se puede usar el método de extensión Generic `AddGrpcClient` en `Startup.ConfigureServices`, especificando la clase de cliente con tipo gRPC y la dirección de servicio:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

El tipo de cliente gRPC se registra como transitorio con la inserción de dependencias (DI). Ahora el cliente se puede insertar y consumir directamente en los tipos creados por DI. ASP.NET Core controladores MVC, SignalR hubs y gRPC Services son lugares en los que se pueden insertar automáticamente los clientes de gRPC:

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>Configuración de HttpClient

`HttpClientFactory` crea el `HttpClient` que usa el cliente de gRPC. Los métodos de `HttpClientFactory` estándar se pueden usar para agregar middleware de solicitud de salida o para configurar el `HttpClientHandler` subyacente del `HttpClient`:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

Para obtener más información, consulte [Make HTTP requests Using IHttpClientFactory](xref:fundamentals/http-requests).

## <a name="configure-channel-and-interceptors"></a>Configurar el canal y los interceptores

los métodos específicos de gRPC están disponibles para:

* Configure el canal subyacente de un cliente de gRPC.
* Agregue `Interceptor` instancias que el cliente usará al realizar llamadas a gRPC.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>Fecha límite y propagación de cancelación

los clientes de gRPC creados por el generador en un servicio de gRPC se pueden configurar con `EnableCallContextPropagation()` para propagar automáticamente la fecha límite y el token de cancelación a las llamadas secundarias. El método de extensión `EnableCallContextPropagation()` está disponible en el paquete NuGet [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .

La propagación de contexto de llamada funciona mediante la lectura de la fecha límite y el token de cancelación del contexto de solicitud gRPC actual y su propagación automática a las llamadas salientes realizadas por el cliente de gRPC. La propagación de contexto de llamada es una excelente manera de garantizar que los escenarios complejos de gRPC anidados siempre propagan la fecha límite y la cancelación.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

Para obtener más información acerca de las fechas límite y la cancelación de RPC, consulte [ciclo de vida de RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
