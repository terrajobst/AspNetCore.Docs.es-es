---
title: Llamar a gRPC Services con el cliente de .NET
author: jamesnk
description: Obtenga información sobre cómo llamar a gRPC Services con el cliente de gRPC de .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829106"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="c443f-103">Llamar a gRPC Services con el cliente de .NET</span><span class="sxs-lookup"><span data-stu-id="c443f-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="c443f-104">Hay una biblioteca de cliente de .NET gRPC disponible en el paquete NuGet de [gRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="c443f-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="c443f-105">En este documento se explica cómo:</span><span class="sxs-lookup"><span data-stu-id="c443f-105">This document explains how to:</span></span>

* <span data-ttu-id="c443f-106">Configure un cliente de gRPC para llamar a los servicios de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c443f-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="c443f-107">Realice llamadas gRPC a los métodos unario, streaming de servidor, streaming de cliente y streaming bidireccional.</span><span class="sxs-lookup"><span data-stu-id="c443f-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="c443f-108">Configurar el cliente de gRPC</span><span class="sxs-lookup"><span data-stu-id="c443f-108">Configure gRPC client</span></span>

<span data-ttu-id="c443f-109">Los clientes gRPC son tipos de cliente concretos que se [generan a partir de archivos *\*.proto*](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="c443f-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="c443f-110">El cliente gRPC concreto tiene métodos que se convierten en el servicio gRPC en el archivo *\*.proto*.</span><span class="sxs-lookup"><span data-stu-id="c443f-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="c443f-111">Se crea un cliente de gRPC a partir de un canal.</span><span class="sxs-lookup"><span data-stu-id="c443f-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="c443f-112">Empiece por usar `GrpcChannel.ForAddress` para crear un canal y, a continuación, use el canal para crear un cliente de gRPC:</span><span class="sxs-lookup"><span data-stu-id="c443f-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="c443f-113">Un canal representa una conexión de larga duración con un servicio de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c443f-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="c443f-114">Cuando se crea un canal, se configura con opciones relacionadas con la llamada a un servicio.</span><span class="sxs-lookup"><span data-stu-id="c443f-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="c443f-115">Por ejemplo, la `HttpClient` utilizada para realizar llamadas, el tamaño máximo del mensaje de envío y recepción y el registro se pueden especificar en `GrpcChannelOptions` y usar con `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="c443f-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="c443f-116">Para obtener una lista completa de opciones, vea [Opciones de configuración de cliente](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="c443f-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="c443f-117">Rendimiento y uso de canal y cliente:</span><span class="sxs-lookup"><span data-stu-id="c443f-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="c443f-118">La creación de un canal puede ser una operación costosa.</span><span class="sxs-lookup"><span data-stu-id="c443f-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="c443f-119">La reutilización de un canal para las llamadas de gRPC proporciona ventajas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="c443f-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="c443f-120">los clientes de gRPC se crean con canales.</span><span class="sxs-lookup"><span data-stu-id="c443f-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="c443f-121">los clientes de gRPC son objetos ligeros y no es necesario que estén almacenados en caché o reutilizados.</span><span class="sxs-lookup"><span data-stu-id="c443f-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="c443f-122">Se pueden crear varios clientes de gRPC a partir de un canal, incluidos distintos tipos de clientes.</span><span class="sxs-lookup"><span data-stu-id="c443f-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="c443f-123">Varios subprocesos pueden usar de forma segura un canal y los clientes creados a partir del canal.</span><span class="sxs-lookup"><span data-stu-id="c443f-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="c443f-124">Los clientes creados desde el canal pueden realizar varias llamadas simultáneas.</span><span class="sxs-lookup"><span data-stu-id="c443f-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="c443f-125">`GrpcChannel.ForAddress` no es la única opción para crear un cliente de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c443f-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="c443f-126">Si está llamando a gRPC Services desde una aplicación ASP.NET Core, considere la [integración de fábrica de cliente de gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="c443f-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="c443f-127">la integración de gRPC con `HttpClientFactory` ofrece una alternativa centralizada para la creación de clientes de gRPC.</span><span class="sxs-lookup"><span data-stu-id="c443f-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="c443f-128">Se requiere configuración adicional para [llamar a los servicios de gRPC inseguros con el cliente de .net](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="c443f-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="c443f-129">Realizar llamadas gRPC</span><span class="sxs-lookup"><span data-stu-id="c443f-129">Make gRPC calls</span></span>

<span data-ttu-id="c443f-130">Una llamada a gRPC se inicia llamando a un método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c443f-130">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="c443f-131">El cliente gRPC controlará la serialización del mensaje y dirigirá la llamada gRPC al servicio correcto.</span><span class="sxs-lookup"><span data-stu-id="c443f-131">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="c443f-132">gRPC tiene distintos tipos de métodos.</span><span class="sxs-lookup"><span data-stu-id="c443f-132">gRPC has different types of methods.</span></span> <span data-ttu-id="c443f-133">La forma de usar el cliente para realizar una llamada gRPC depende del tipo de método al que se está llamando.</span><span class="sxs-lookup"><span data-stu-id="c443f-133">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="c443f-134">Los tipos de método gRPC son:</span><span class="sxs-lookup"><span data-stu-id="c443f-134">The gRPC method types are:</span></span>

* <span data-ttu-id="c443f-135">Unario</span><span class="sxs-lookup"><span data-stu-id="c443f-135">Unary</span></span>
* <span data-ttu-id="c443f-136">Streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="c443f-136">Server streaming</span></span>
* <span data-ttu-id="c443f-137">Streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="c443f-137">Client streaming</span></span>
* <span data-ttu-id="c443f-138">Streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="c443f-138">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="c443f-139">Llamada unaria</span><span class="sxs-lookup"><span data-stu-id="c443f-139">Unary call</span></span>

<span data-ttu-id="c443f-140">Una llamada unaria comienza con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c443f-140">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="c443f-141">Cuando finaliza el servicio, se devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="c443f-141">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="c443f-142">Cada método de servicio unario del archivo *\*. proto* dará como resultado dos métodos .net en el tipo de cliente gRPC concreto para llamar al método: un método asincrónico y un método de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="c443f-142">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="c443f-143">Por ejemplo, en `GreeterClient` hay dos maneras de llamar a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="c443f-143">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="c443f-144">`GreeterClient.SayHelloAsync`: llama a `Greeter.SayHello` servicio de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="c443f-144">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="c443f-145">Se puede esperar.</span><span class="sxs-lookup"><span data-stu-id="c443f-145">Can be awaited.</span></span>
* <span data-ttu-id="c443f-146">`GreeterClient.SayHello`: llama a `Greeter.SayHello` servicio y se bloquea hasta que se completa.</span><span class="sxs-lookup"><span data-stu-id="c443f-146">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="c443f-147">No se usa en código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="c443f-147">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="c443f-148">Llamada de streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="c443f-148">Server streaming call</span></span>

<span data-ttu-id="c443f-149">Una llamada de streaming de servidor se inicia con el cliente que envía un mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="c443f-149">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="c443f-150">`ResponseStream.MoveNext()` Lee los mensajes transmitidos desde el servicio.</span><span class="sxs-lookup"><span data-stu-id="c443f-150">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="c443f-151">La llamada de streaming del servidor se completa cuando `ResponseStream.MoveNext()` devuelve `false`.</span><span class="sxs-lookup"><span data-stu-id="c443f-151">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="c443f-152">Si usa C# 8 o posterior, se puede usar la sintaxis de `await foreach` para leer los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c443f-152">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="c443f-153">El método de extensión `IAsyncStreamReader<T>.ReadAllAsync()` Lee todos los mensajes de la secuencia de respuesta:</span><span class="sxs-lookup"><span data-stu-id="c443f-153">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="c443f-154">Llamada de streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="c443f-154">Client streaming call</span></span>

<span data-ttu-id="c443f-155">Una llamada de streaming de cliente se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="c443f-155">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c443f-156">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="c443f-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c443f-157">Cuando el cliente ha terminado de enviar mensajes `RequestStream.CompleteAsync` se debe llamar a para notificar al servicio.</span><span class="sxs-lookup"><span data-stu-id="c443f-157">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="c443f-158">La llamada finaliza cuando el servicio devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="c443f-158">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="c443f-159">Llamada de streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="c443f-159">Bi-directional streaming call</span></span>

<span data-ttu-id="c443f-160">Una llamada de streaming bidireccional se inicia *sin* que el cliente envíe un mensaje.</span><span class="sxs-lookup"><span data-stu-id="c443f-160">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c443f-161">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="c443f-161">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c443f-162">Se puede obtener acceso a los mensajes transmitidos desde el servicio con `ResponseStream.MoveNext()` o `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="c443f-162">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="c443f-163">La llamada de streaming bidireccional se completa cuando el `ResponseStream` no tiene más mensajes.</span><span class="sxs-lookup"><span data-stu-id="c443f-163">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="c443f-164">Durante una llamada de streaming bidireccional, el cliente y el servicio pueden enviar mensajes entre sí en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="c443f-164">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="c443f-165">La mejor lógica de cliente para interactuar con una llamada bidireccional varía en función de la lógica del servicio.</span><span class="sxs-lookup"><span data-stu-id="c443f-165">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c443f-166">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c443f-166">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
