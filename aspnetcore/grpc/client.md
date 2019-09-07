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
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="dc9fe-103">Llamar a gRPC Services con el cliente de .NET</span><span class="sxs-lookup"><span data-stu-id="dc9fe-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="dc9fe-104">Hay una biblioteca de cliente de .NET gRPC disponible en el paquete NuGet de [gRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="dc9fe-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="dc9fe-105">En este documento se explica cómo:</span><span class="sxs-lookup"><span data-stu-id="dc9fe-105">This document explains how to:</span></span>

* <span data-ttu-id="dc9fe-106">Configure un cliente de gRPC para llamar a los servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="dc9fe-107">Realice llamadas gRPC a los métodos unario, streaming de servidor, streaming de cliente y streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="dc9fe-108">Configurar el cliente de gRPC</span><span class="sxs-lookup"><span data-stu-id="dc9fe-108">Configure gRPC client</span></span>

<span data-ttu-id="dc9fe-109">los clientes de gRPC son tipos de cliente concretos que se [generan a partir de  *\*archivos. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="dc9fe-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="dc9fe-110">El cliente gRPC concreto tiene métodos que se convierten en el servicio gRPC en el  *\*archivo. proto* .</span><span class="sxs-lookup"><span data-stu-id="dc9fe-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="dc9fe-111">Se crea un cliente de gRPC a partir de un canal.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="dc9fe-112">Empiece por usar `GrpcChannel.ForAddress` para crear un canal y, a continuación, use el canal para crear un cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="dc9fe-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="dc9fe-113">Un canal representa una conexión de larga duración con un servicio de gRPC.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="dc9fe-114">Cuando se crea un canal, se configura con las opciones relacionadas con la llamada a un servicio.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="dc9fe-115">Por ejemplo, el `HttpClient` que se usa para realizar llamadas, el tamaño máximo del mensaje de envío y recepción y el registro `GrpcChannelOptions` se pueden especificar `GrpcChannel.ForAddress`en y usar con.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="dc9fe-116">Para obtener una lista completa de opciones, vea [Opciones de configuración de cliente](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="dc9fe-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="dc9fe-117">La creación de un canal puede ser una operación costosa y volver a usar un canal para las llamadas de gRPC ofrece ventajas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="dc9fe-118">Se pueden crear varios clientes de gRPC concretos a partir de un canal, incluidos distintos tipos de clientes.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="dc9fe-119">Los tipos de cliente gRPC concretos son objetos ligeros y se pueden crear cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="dc9fe-120">`GrpcChannel.ForAddress`no es la única opción para crear un cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="dc9fe-121">Si va a llamar a gRPC Services desde una aplicación ASP.NET Core, considere la [integración de fábrica de cliente de gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="dc9fe-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="dc9fe-122">la integración de `HttpClientFactory` gRPC con ofrece una alternativa centralizada a la creación de clientes de gRPC.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="dc9fe-123">Realizar llamadas gRPC</span><span class="sxs-lookup"><span data-stu-id="dc9fe-123">Make gRPC calls</span></span>

<span data-ttu-id="dc9fe-124">Una llamada a gRPC se inicia llamando a un método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-124">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="dc9fe-125">El cliente gRPC controlará la serialización del mensaje y dirigirá la llamada gRPC al servicio correcto.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-125">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="dc9fe-126">gRPC tiene distintos tipos de métodos.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-126">gRPC has different types of methods.</span></span> <span data-ttu-id="dc9fe-127">La forma de usar el cliente para realizar una llamada gRPC depende del tipo de método al que se está llamando.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-127">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="dc9fe-128">Los tipos de método gRPC son:</span><span class="sxs-lookup"><span data-stu-id="dc9fe-128">The gRPC method types are:</span></span>

* <span data-ttu-id="dc9fe-129">Unario</span><span class="sxs-lookup"><span data-stu-id="dc9fe-129">Unary</span></span>
* <span data-ttu-id="dc9fe-130">Streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="dc9fe-130">Server streaming</span></span>
* <span data-ttu-id="dc9fe-131">Streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="dc9fe-131">Client streaming</span></span>
* <span data-ttu-id="dc9fe-132">Streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="dc9fe-132">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="dc9fe-133">Llamada unaria</span><span class="sxs-lookup"><span data-stu-id="dc9fe-133">Unary call</span></span>

<span data-ttu-id="dc9fe-134">Una llamada unaria comienza con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-134">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="dc9fe-135">Cuando finaliza el servicio, se devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-135">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="dc9fe-136">Cada método de  *\** servicio unario del archivo. proto dará como resultado dos métodos .net en el tipo de cliente gRPC concreto para llamar al método: un método asincrónico y un método de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-136">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="dc9fe-137">Por ejemplo, en `GreeterClient` hay dos maneras de llamar a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="dc9fe-137">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="dc9fe-138">`GreeterClient.SayHelloAsync`: llama `Greeter.SayHello` al servicio de de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-138">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="dc9fe-139">Se puede esperar.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-139">Can be awaited.</span></span>
* <span data-ttu-id="dc9fe-140">`GreeterClient.SayHello`: llama `Greeter.SayHello` al servicio y se bloquea hasta que se completa.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-140">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="dc9fe-141">No se usa en código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-141">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="dc9fe-142">Llamada de streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="dc9fe-142">Server streaming call</span></span>

<span data-ttu-id="dc9fe-143">Una llamada de streaming de servidor se inicia con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-143">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="dc9fe-144">`ResponseStream.MoveNext()`Lee los mensajes transmitidos desde el servicio.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-144">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="dc9fe-145">La llamada de streaming del servidor `ResponseStream.MoveNext()` se `false`completa cuando devuelve.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-145">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="dc9fe-146">Si usa C# 8 o posterior, se puede usar `await foreach` la sintaxis para leer los mensajes.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-146">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="dc9fe-147">El `IAsyncStreamReader<T>.ReadAllAsync()` método de extensión Lee todos los mensajes de la secuencia de respuesta:</span><span class="sxs-lookup"><span data-stu-id="dc9fe-147">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="dc9fe-148">Llamada de streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="dc9fe-148">Client streaming call</span></span>

<span data-ttu-id="dc9fe-149">Una llamada de streaming de cliente se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-149">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="dc9fe-150">El cliente puede elegir enviar mensajes de envío con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-150">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="dc9fe-151">Cuando el cliente ha terminado de enviar `RequestStream.CompleteAsync` mensajes, se debe llamar a para notificar al servicio.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-151">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="dc9fe-152">La llamada finaliza cuando el servicio devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-152">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="dc9fe-153">Llamada de streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="dc9fe-153">Bi-directional streaming call</span></span>

<span data-ttu-id="dc9fe-154">Una llamada de streaming bidireccional se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-154">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="dc9fe-155">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-155">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="dc9fe-156">Se puede obtener acceso a los mensajes transmitidos `ResponseStream.MoveNext()` desde `ResponseStream.ReadAllAsync()`el servicio con o.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-156">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="dc9fe-157">La llamada de streaming bidireccional se completa cuando el `ResponseStream` no tiene más mensajes.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-157">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="dc9fe-158">Durante una llamada de streaming bidireccional, el cliente y el servicio pueden enviar mensajes entre sí en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-158">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="dc9fe-159">La mejor lógica de cliente para interactuar con una llamada bidireccional varía en función de la lógica del servicio.</span><span class="sxs-lookup"><span data-stu-id="dc9fe-159">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc9fe-160">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dc9fe-160">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
