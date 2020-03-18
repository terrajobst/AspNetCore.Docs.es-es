---
title: Llamada a servicios gRPC con el cliente .NET
author: jamesnk
description: Obtenga información sobre cómo llamar a servicios gRPC con el cliente gRPC de .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78650807"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="05195-103">Llamada a servicios gRPC con el cliente .NET</span><span class="sxs-lookup"><span data-stu-id="05195-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="05195-104">Hay una biblioteca cliente gRPC de .NET disponible en el paquete NuGet [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="05195-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="05195-105">En este documento se explica cómo:</span><span class="sxs-lookup"><span data-stu-id="05195-105">This document explains how to:</span></span>

* <span data-ttu-id="05195-106">Configurar un cliente gRPC para llamar a servicios gRPC.</span><span class="sxs-lookup"><span data-stu-id="05195-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="05195-107">Realizar llamadas gRPC a los métodos de streaming unario, de servidor, de cliente y bidireccional.</span><span class="sxs-lookup"><span data-stu-id="05195-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="05195-108">Configuración del cliente gRPC</span><span class="sxs-lookup"><span data-stu-id="05195-108">Configure gRPC client</span></span>

<span data-ttu-id="05195-109">Los clientes gRPC son tipos de cliente concretos que se [generan a partir de archivos *\*.proto*](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="05195-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="05195-110">El cliente gRPC concreto tiene métodos que se convierten en el servicio gRPC en el archivo *\*.proto*.</span><span class="sxs-lookup"><span data-stu-id="05195-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="05195-111">Un cliente gRPC se crea a partir de un canal.</span><span class="sxs-lookup"><span data-stu-id="05195-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="05195-112">Para empezar, use `GrpcChannel.ForAddress` para crear un canal y, después, use el canal para crear un cliente gRPC:</span><span class="sxs-lookup"><span data-stu-id="05195-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="05195-113">Un canal representa una conexión de larga duración con un servicio gRPC.</span><span class="sxs-lookup"><span data-stu-id="05195-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="05195-114">Cuando se crea un canal, se configura con opciones relacionadas con la llamada a un servicio.</span><span class="sxs-lookup"><span data-stu-id="05195-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="05195-115">Por ejemplo, el elemento `HttpClient` que se usa para realizar llamadas, el tamaño máximo del mensaje de envío y recepción, y el registro se pueden especificar en `GrpcChannelOptions` y usar con `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="05195-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="05195-116">Para obtener una lista completa de las opciones, vea [Opciones de configuración de cliente](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="05195-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="05195-117">Rendimiento y uso de canales y clientes:</span><span class="sxs-lookup"><span data-stu-id="05195-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="05195-118">La creación de un canal puede ser una operación costosa.</span><span class="sxs-lookup"><span data-stu-id="05195-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="05195-119">La reutilización de un canal para las llamadas de gRPC proporciona ventajas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="05195-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="05195-120">Los clientes gRPC se crean con canales.</span><span class="sxs-lookup"><span data-stu-id="05195-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="05195-121">Los clientes gRPC son objetos ligeros y no es necesario que se almacenen en caché ni reutilizarlos.</span><span class="sxs-lookup"><span data-stu-id="05195-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="05195-122">Se pueden crear varios clientes gRPC a partir de un canal, incluidos distintos tipos de clientes.</span><span class="sxs-lookup"><span data-stu-id="05195-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="05195-123">Varios subprocesos pueden usar de forma segura un canal y los clientes creados a partir del canal.</span><span class="sxs-lookup"><span data-stu-id="05195-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="05195-124">Los clientes creados a partir del canal pueden realizar varias llamadas simultáneas.</span><span class="sxs-lookup"><span data-stu-id="05195-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="05195-125">`GrpcChannel.ForAddress` no es la única opción para crear un cliente gRPC.</span><span class="sxs-lookup"><span data-stu-id="05195-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="05195-126">Si va a llamar a servicios gRPC desde una aplicación ASP.NET Core, considere la posibilidad de usar la [integración de fábrica de cliente de gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="05195-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="05195-127">La integración de gRPC con `HttpClientFactory` ofrece una alternativa centralizada a la creación de clientes gRPC.</span><span class="sxs-lookup"><span data-stu-id="05195-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="05195-128">Se requiere configuración adicional para [llamar a servicios gRPC no seguros con el cliente de .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="05195-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="05195-129">La llamada a gRPC a través de HTTP/2 con `Grpc.Net.Client` no se admite actualmente en Xamarin.</span><span class="sxs-lookup"><span data-stu-id="05195-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="05195-130">Estamos trabajando para mejorar la compatibilidad con HTTP/2 en una versión futura de Xamarin.</span><span class="sxs-lookup"><span data-stu-id="05195-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="05195-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) y [gRPC-Web](xref:grpc/browser) son alternativas viables que funcionan en la actualidad.</span><span class="sxs-lookup"><span data-stu-id="05195-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="05195-132">Realización de llamadas gRPC</span><span class="sxs-lookup"><span data-stu-id="05195-132">Make gRPC calls</span></span>

<span data-ttu-id="05195-133">Una llamada gRPC se inicia mediante la llamada a un método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="05195-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="05195-134">El cliente gRPC controlará la serialización de mensajes y dirigirá la llamada gRPC al servicio correcto.</span><span class="sxs-lookup"><span data-stu-id="05195-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="05195-135">gRPC tiene distintos tipos de métodos.</span><span class="sxs-lookup"><span data-stu-id="05195-135">gRPC has different types of methods.</span></span> <span data-ttu-id="05195-136">La forma de usar el cliente para realizar una llamada gRPC depende del tipo de método al que se llame.</span><span class="sxs-lookup"><span data-stu-id="05195-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="05195-137">Los tipos de métodos gRPC son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="05195-137">The gRPC method types are:</span></span>

* <span data-ttu-id="05195-138">Unario</span><span class="sxs-lookup"><span data-stu-id="05195-138">Unary</span></span>
* <span data-ttu-id="05195-139">Streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="05195-139">Server streaming</span></span>
* <span data-ttu-id="05195-140">Streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="05195-140">Client streaming</span></span>
* <span data-ttu-id="05195-141">Streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="05195-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="05195-142">Llamada unaria</span><span class="sxs-lookup"><span data-stu-id="05195-142">Unary call</span></span>

<span data-ttu-id="05195-143">Una llamada unaria comienza con el envío de un mensaje de solicitud por parte del cliente.</span><span class="sxs-lookup"><span data-stu-id="05195-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="05195-144">Cuando finaliza el servicio, se devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="05195-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="05195-145">Cada método de servicio unario del archivo *\*.proto* dará como resultado dos métodos de .NET en el tipo de cliente gRPC concreto para llamar al método: un método asincrónico y un método de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="05195-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="05195-146">Por ejemplo, en `GreeterClient` hay dos maneras de llamar a `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="05195-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="05195-147">`GreeterClient.SayHelloAsync`: llama al servicio `Greeter.SayHello` de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="05195-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="05195-148">Se le puede aplicar espera.</span><span class="sxs-lookup"><span data-stu-id="05195-148">Can be awaited.</span></span>
* <span data-ttu-id="05195-149">`GreeterClient.SayHello`: llama al servicio `Greeter.SayHello` y se bloquea hasta que se completa.</span><span class="sxs-lookup"><span data-stu-id="05195-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="05195-150">No se debe usar en código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="05195-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="05195-151">Llamada de streaming de servidor</span><span class="sxs-lookup"><span data-stu-id="05195-151">Server streaming call</span></span>

<span data-ttu-id="05195-152">Una llamada de streaming de servidor comienza con el envío de un mensaje de solicitud por parte del cliente.</span><span class="sxs-lookup"><span data-stu-id="05195-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="05195-153">`ResponseStream.MoveNext()` lee los mensajes transmitidos desde el servicio.</span><span class="sxs-lookup"><span data-stu-id="05195-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="05195-154">La llamada de streaming del servidor se completa cuando `ResponseStream.MoveNext()` devuelve `false`.</span><span class="sxs-lookup"><span data-stu-id="05195-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="05195-155">Si usa C# 8 o una versión posterior, se puede usar la sintaxis de `await foreach` para leer los mensajes.</span><span class="sxs-lookup"><span data-stu-id="05195-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="05195-156">El método de extensión `IAsyncStreamReader<T>.ReadAllAsync()` lee todos los mensajes de la secuencia de respuesta:</span><span class="sxs-lookup"><span data-stu-id="05195-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="05195-157">Llamada de streaming de cliente</span><span class="sxs-lookup"><span data-stu-id="05195-157">Client streaming call</span></span>

<span data-ttu-id="05195-158">Una llamada de streaming de cliente comienza *sin* el envío de un mensaje por parte del cliente.</span><span class="sxs-lookup"><span data-stu-id="05195-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="05195-159">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="05195-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="05195-160">Cuando el cliente ha terminado de enviar mensajes se debe llamar a `RequestStream.CompleteAsync` para notificar al servicio.</span><span class="sxs-lookup"><span data-stu-id="05195-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="05195-161">La llamada finaliza cuando el servicio devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="05195-161">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="05195-162">Llamada de streaming bidireccional</span><span class="sxs-lookup"><span data-stu-id="05195-162">Bi-directional streaming call</span></span>

<span data-ttu-id="05195-163">Una llamada de streaming bidireccional comienza *sin* el envío de un mensaje por parte del cliente.</span><span class="sxs-lookup"><span data-stu-id="05195-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="05195-164">El cliente puede elegir enviar mensajes con `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="05195-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="05195-165">Se puede acceder a los mensajes transmitidos desde el servicio con `ResponseStream.MoveNext()` o `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="05195-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="05195-166">La llamada de streaming bidireccional se completa cuando `ResponseStream` no tiene más mensajes.</span><span class="sxs-lookup"><span data-stu-id="05195-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="05195-167">Durante una llamada de streaming bidireccional, el cliente y el servicio se pueden enviar mensajes entre sí en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="05195-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="05195-168">La mejor lógica de cliente para interactuar con una llamada bidireccional varía en función de la lógica del servicio.</span><span class="sxs-lookup"><span data-stu-id="05195-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="05195-169">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="05195-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
