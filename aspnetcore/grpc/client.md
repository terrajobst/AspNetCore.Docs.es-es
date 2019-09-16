---
title: Llamar a gRPC Services con el cliente de .NET
author: jamesnk
description: Obtenga información sobre cómo llamar a gRPC Services con el cliente de gRPC de .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 56f79b303a8d53699e8eb6156d328c0da1259416
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011145"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="e8631-103">Llamar a gRPC Services con el cliente de .NET</span><span class="sxs-lookup"><span data-stu-id="e8631-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="e8631-104">Hay una biblioteca de cliente de .NET gRPC disponible en el paquete NuGet de [gRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="e8631-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="e8631-105">En este documento se explica cómo:</span><span class="sxs-lookup"><span data-stu-id="e8631-105">This document explains how to:</span></span>

* <span data-ttu-id="e8631-106">Configure un cliente de gRPC para llamar a los servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="e8631-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="e8631-107">Realice llamadas gRPC a los métodos unario, streaming de servidor, streaming de cliente y streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="e8631-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="e8631-108">Configurar el cliente de gRPC</span><span class="sxs-lookup"><span data-stu-id="e8631-108">Configure gRPC client</span></span>

<span data-ttu-id="e8631-109">los clientes de gRPC son tipos de cliente concretos que se [generan a partir de  *\*archivos. proto* ](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="e8631-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="e8631-110">El cliente gRPC concreto tiene métodos que se convierten en el servicio gRPC en el  *\*archivo. proto* .</span><span class="sxs-lookup"><span data-stu-id="e8631-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="e8631-111">Se crea un cliente de gRPC a partir de un canal.</span><span class="sxs-lookup"><span data-stu-id="e8631-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="e8631-112">Empiece por usar `GrpcChannel.ForAddress` para crear un canal y, a continuación, use el canal para crear un cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="e8631-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="e8631-113">Un canal representa una conexión de larga duración con un servicio de gRPC.</span><span class="sxs-lookup"><span data-stu-id="e8631-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="e8631-114">Cuando se crea un canal, se configura con las opciones relacionadas con la llamada a un servicio.</span><span class="sxs-lookup"><span data-stu-id="e8631-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="e8631-115">Por ejemplo, el `HttpClient` que se usa para realizar llamadas, el tamaño máximo del mensaje de envío y recepción y el registro `GrpcChannelOptions` se pueden especificar `GrpcChannel.ForAddress`en y usar con.</span><span class="sxs-lookup"><span data-stu-id="e8631-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="e8631-116">Para obtener una lista completa de opciones, vea [Opciones de configuración de cliente](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="e8631-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="e8631-117">La creación de un canal puede ser una operación costosa y volver a usar un canal para las llamadas de gRPC ofrece ventajas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="e8631-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="e8631-118">Se pueden crear varios clientes de gRPC concretos a partir de un canal, incluidos distintos tipos de clientes.</span><span class="sxs-lookup"><span data-stu-id="e8631-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="e8631-119">Los tipos de cliente gRPC concretos son objetos ligeros y se pueden crear cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="e8631-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="e8631-120">`GrpcChannel.ForAddress`no es la única opción para crear un cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="e8631-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="e8631-121">Si va a llamar a gRPC Services desde una aplicación ASP.NET Core, considere la [integración de fábrica de cliente de gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="e8631-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="e8631-122">la integración de `HttpClientFactory` gRPC con ofrece una alternativa centralizada a la creación de clientes de gRPC.</span><span class="sxs-lookup"><span data-stu-id="e8631-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="e8631-123">Se requiere configuración adicional para [llamar a los servicios de gRPC inseguros con el cliente de .net](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="e8631-123">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="e8631-124">Realizar llamadas gRPC</span><span class="sxs-lookup"><span data-stu-id="e8631-124">Make gRPC calls</span></span>

<span data-ttu-id="e8631-125">Una llamada a gRPC se inicia llamando a un método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="e8631-125">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="e8631-126">El cliente gRPC controlará la serialización del mensaje y dirigirá la llamada gRPC al servicio correcto.</span><span class="sxs-lookup"><span data-stu-id="e8631-126">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="e8631-127">gRPC tiene distintos tipos de métodos.</span><span class="sxs-lookup"><span data-stu-id="e8631-127">gRPC has different types of methods.</span></span> <span data-ttu-id="e8631-128">La forma de usar el cliente para realizar una llamada gRPC depende del tipo de método al que se está llamando.</span><span class="sxs-lookup"><span data-stu-id="e8631-128">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="e8631-129">Los tipos de método gRPC son:</span><span class="sxs-lookup"><span data-stu-id="e8631-129">The gRPC method types are:</span></span>

* <span data-ttu-id="e8631-130">Unario</span><span class="sxs-lookup"><span data-stu-id="e8631-130">Unary</span></span>
* <span data-ttu-id="e8631-131">Streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="e8631-131">Server streaming</span></span>
* <span data-ttu-id="e8631-132">Streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="e8631-132">Client streaming</span></span>
* <span data-ttu-id="e8631-133">Streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="e8631-133">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="e8631-134">Llamada unaria</span><span class="sxs-lookup"><span data-stu-id="e8631-134">Unary call</span></span>

<span data-ttu-id="e8631-135">Una llamada unaria comienza con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="e8631-135">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="e8631-136">Cuando finaliza el servicio, se devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="e8631-136">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="e8631-137">Cada método de  *\** servicio unario del archivo. proto dará como resultado dos métodos .net en el tipo de cliente gRPC concreto para llamar al método: un método asincrónico y un método de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="e8631-137">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="e8631-138">Por ejemplo, en `GreeterClient` hay dos maneras de llamar a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="e8631-138">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="e8631-139">`GreeterClient.SayHelloAsync`: llama `Greeter.SayHello` al servicio de de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="e8631-139">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="e8631-140">Se puede esperar.</span><span class="sxs-lookup"><span data-stu-id="e8631-140">Can be awaited.</span></span>
* <span data-ttu-id="e8631-141">`GreeterClient.SayHello`: llama `Greeter.SayHello` al servicio y se bloquea hasta que se completa.</span><span class="sxs-lookup"><span data-stu-id="e8631-141">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="e8631-142">No se usa en código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="e8631-142">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="e8631-143">Llamada de streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="e8631-143">Server streaming call</span></span>

<span data-ttu-id="e8631-144">Una llamada de streaming de servidor se inicia con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="e8631-144">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="e8631-145">`ResponseStream.MoveNext()`Lee los mensajes transmitidos desde el servicio.</span><span class="sxs-lookup"><span data-stu-id="e8631-145">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="e8631-146">La llamada de streaming del servidor `ResponseStream.MoveNext()` se `false`completa cuando devuelve.</span><span class="sxs-lookup"><span data-stu-id="e8631-146">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="e8631-147">Si usa C# 8 o posterior, se puede usar `await foreach` la sintaxis para leer los mensajes.</span><span class="sxs-lookup"><span data-stu-id="e8631-147">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="e8631-148">El `IAsyncStreamReader<T>.ReadAllAsync()` método de extensión Lee todos los mensajes de la secuencia de respuesta:</span><span class="sxs-lookup"><span data-stu-id="e8631-148">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="e8631-149">Llamada de streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="e8631-149">Client streaming call</span></span>

<span data-ttu-id="e8631-150">Una llamada de streaming de cliente se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="e8631-150">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="e8631-151">El cliente puede elegir enviar mensajes de envío con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="e8631-151">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="e8631-152">Cuando el cliente ha terminado de enviar `RequestStream.CompleteAsync` mensajes, se debe llamar a para notificar al servicio.</span><span class="sxs-lookup"><span data-stu-id="e8631-152">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="e8631-153">La llamada finaliza cuando el servicio devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="e8631-153">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="e8631-154">Llamada de streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="e8631-154">Bi-directional streaming call</span></span>

<span data-ttu-id="e8631-155">Una llamada de streaming bidireccional se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="e8631-155">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="e8631-156">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="e8631-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="e8631-157">Se puede obtener acceso a los mensajes transmitidos `ResponseStream.MoveNext()` desde `ResponseStream.ReadAllAsync()`el servicio con o.</span><span class="sxs-lookup"><span data-stu-id="e8631-157">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="e8631-158">La llamada de streaming bidireccional se completa cuando el `ResponseStream` no tiene más mensajes.</span><span class="sxs-lookup"><span data-stu-id="e8631-158">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="e8631-159">Durante una llamada de streaming bidireccional, el cliente y el servicio pueden enviar mensajes entre sí en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="e8631-159">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="e8631-160">La mejor lógica de cliente para interactuar con una llamada bidireccional varía en función de la lógica del servicio.</span><span class="sxs-lookup"><span data-stu-id="e8631-160">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8631-161">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e8631-161">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
