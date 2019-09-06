---
title: integración de gRPC Client Factory en .NET Core
author: jamesnk
description: Aprenda a crear clientes de gRPC mediante el generador de cliente.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/clientfactory
ms.openlocfilehash: a52fd397a7ed3e327938b0847af7f4e6a4a79400
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310611"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>integración de gRPC Client Factory en .NET Core

la integración de `HttpClientFactory` gRPC con ofrece una manera centralizada de crear clientes de gRPC. Se puede usar como alternativa a [la configuración de instancias de cliente de gRPC independientes](xref:grpc/client). La integración de fábrica está disponible en el paquete NuGet [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .

El generador ofrece las siguientes ventajas:

* Proporciona una ubicación central para configurar las instancias de cliente gRPC lógicas
* Administra la duración del`HttpClientMessageHandler`
* Propagación automática de fecha límite y cancelación en un servicio de gRPC de ASP.NET Core

## <a name="register-grpc-clients"></a>Registro de clientes de gRPC

Para registrar un cliente de gRPC, se `AddGrpcClient` puede usar el método de extensión `Startup.ConfigureServices`genérico en, especificando la clase de cliente con tipo de gRPC y la dirección de servicio:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("http://localhost:5001");
});
```

El tipo de cliente gRPC se registra como transitorio con la inserción de dependencias (DI). Ahora el cliente se puede insertar y consumir directamente en los tipos creados por DI. Los controladores de ASP.NET Core MVC, los concentradores de Signalr y los servicios de gRPC son lugares donde se pueden insertar automáticamente los clientes de gRPC:

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

`HttpClientFactory`crea el `HttpClient` que usa el cliente de gRPC. Los `HttpClientFactory` métodos estándar se pueden usar para agregar middleware de solicitud de salida o para `HttpClientHandler` configurar el `HttpClient`subyacente de:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.BaseAddress = new Uri("http://localhost:5001");
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
        o.Address = new Uri("http://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>Fecha límite y propagación de cancelación

los clientes de gRPC creados por el generador en un servicio de gRPC se `EnableCallContextPropagation()` pueden configurar con para propagar automáticamente la fecha límite y el token de cancelación a las llamadas secundarias. El `EnableCallContextPropagation()` método de extensión está disponible en el paquete NuGet [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .

La propagación de contexto de llamada funciona mediante la lectura de la fecha límite y el token de cancelación del contexto de solicitud gRPC actual y su propagación automática a las llamadas salientes realizadas por el cliente de gRPC. La propagación de contexto de llamada es una excelente manera de garantizar que los escenarios complejos de gRPC anidados siempre propagan la fecha límite y la cancelación.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("http://localhost:5001");
    })
    .EnableCallContextPropagation();
```

Para obtener más información acerca de las fechas límite y la cancelación de RPC, consulte [ciclo de vida de RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
