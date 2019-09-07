---
title: Llamar a gRPC Services con el cliente de .NET
author: jamesnk
description: Obtenga información sobre cómo llamar a gRPC Services con el cliente de gRPC de .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 27e4b3e7307ae49bacb01a46fbc1b55b6967c7a0
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773695"
---
# <a name="call-grpc-services-with-the-net-client"></a>Llamar a gRPC Services con el cliente de .NET

Hay una biblioteca de cliente de .NET gRPC disponible en el paquete NuGet de [gRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) . En este documento se explica cómo:

* Configure un cliente de gRPC para llamar a los servicios de gRPC.
* Realice llamadas gRPC a los métodos unario, streaming de servidor, streaming de cliente y streaming bidireccional.

## <a name="configure-grpc-client"></a>Configurar el cliente de gRPC

los clientes de gRPC son tipos de cliente concretos que se [generan a partir de  *\*archivos. proto* ](xref:grpc/basics#generated-c-assets). El cliente gRPC concreto tiene métodos que se convierten en el servicio gRPC en el  *\*archivo. proto* .

Se crea un cliente de gRPC a partir de un canal. Empiece por usar `GrpcChannel.ForAddress` para crear un canal y, a continuación, use el canal para crear un cliente de gRPC:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Un canal representa una conexión de larga duración con un servicio de gRPC. Cuando se crea un canal, se configura con las opciones relacionadas con la llamada a un servicio. Por ejemplo, el `HttpClient` que se usa para realizar llamadas, el tamaño máximo del mensaje de envío y recepción y el registro `GrpcChannelOptions` se pueden especificar `GrpcChannel.ForAddress`en y usar con. Para obtener una lista completa de opciones, vea [Opciones de configuración de cliente](xref:grpc/configuration#configure-client-options).

La creación de un canal puede ser una operación costosa y volver a usar un canal para las llamadas de gRPC ofrece ventajas de rendimiento. Se pueden crear varios clientes de gRPC concretos a partir de un canal, incluidos distintos tipos de clientes. Los tipos de cliente gRPC concretos son objetos ligeros y se pueden crear cuando sea necesario.

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

`GrpcChannel.ForAddress`no es la única opción para crear un cliente de gRPC. Si va a llamar a gRPC Services desde una aplicación ASP.NET Core, considere la [integración de fábrica de cliente de gRPC](xref:grpc/clientfactory). la integración de `HttpClientFactory` gRPC con ofrece una alternativa centralizada a la creación de clientes de gRPC.

## <a name="make-grpc-calls"></a>Realizar llamadas gRPC

Una llamada a gRPC se inicia llamando a un método en el cliente. El cliente gRPC controlará la serialización del mensaje y dirigirá la llamada gRPC al servicio correcto.

gRPC tiene distintos tipos de métodos. La forma de usar el cliente para realizar una llamada gRPC depende del tipo de método al que se está llamando. Los tipos de método gRPC son:

* Unario
* Streaming de servidor
* Streaming de cliente
* Streaming bidireccional

### <a name="unary-call"></a>Llamada unaria

Una llamada unaria comienza con el cliente que envía un mensaje de solicitud. Cuando finaliza el servicio, se devuelve un mensaje de respuesta.

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

Cada método de  *\** servicio unario del archivo. proto dará como resultado dos métodos .net en el tipo de cliente gRPC concreto para llamar al método: un método asincrónico y un método de bloqueo. Por ejemplo, en `GreeterClient` hay dos maneras de llamar a `SayHello`:

* `GreeterClient.SayHelloAsync`: llama `Greeter.SayHello` al servicio de de forma asincrónica. Se puede esperar.
* `GreeterClient.SayHello`: llama `Greeter.SayHello` al servicio y se bloquea hasta que se completa. No se usa en código asincrónico.

### <a name="server-streaming-call"></a>Llamada de streaming de servidor

Una llamada de streaming de servidor se inicia con el cliente que envía un mensaje de solicitud. `ResponseStream.MoveNext()`Lee los mensajes transmitidos desde el servicio. La llamada de streaming del servidor `ResponseStream.MoveNext()` se `false`completa cuando devuelve.

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

Si usa C# 8 o posterior, se puede usar `await foreach` la sintaxis para leer los mensajes. El `IAsyncStreamReader<T>.ReadAllAsync()` método de extensión Lee todos los mensajes de la secuencia de respuesta:

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a>Llamada de streaming de cliente

Una llamada de streaming de cliente se inicia *sin* que el cliente envíe un mensaje. El cliente puede elegir enviar mensajes de envío con `RequestStream.WriteAsync`. Cuando el cliente ha terminado de enviar `RequestStream.CompleteAsync` mensajes, se debe llamar a para notificar al servicio. La llamada finaliza cuando el servicio devuelve un mensaje de respuesta.

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a>Llamada de streaming bidireccional

Una llamada de streaming bidireccional se inicia *sin* que el cliente envíe un mensaje. El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`. Se puede obtener acceso a los mensajes transmitidos `ResponseStream.MoveNext()` desde `ResponseStream.ReadAllAsync()`el servicio con o. La llamada de streaming bidireccional se completa cuando el `ResponseStream` no tiene más mensajes.

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

Durante una llamada de streaming bidireccional, el cliente y el servicio pueden enviar mensajes entre sí en cualquier momento. La mejor lógica de cliente para interactuar con una llamada bidireccional varía en función de la lógica del servicio.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
