---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo transmitir datos entre el cliente y el servidor.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: 8f39fdfa45766b5bbec572970f009abefefdc419
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897202"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="32c6a-103">Usar la transmisión por secuencias en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="32c6a-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="32c6a-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="32c6a-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32c6a-105">SignalR de ASP.NET Core admite la transmisión del cliente al servidor y del servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="32c6a-106">Esto es útil para escenarios que llegan los fragmentos de datos con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="32c6a-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="32c6a-107">La transmisión por secuencias, significará que se ha enviado cada fragmento para el cliente o servidor tan pronto como se convierte en disponible, en lugar de esperar todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="32c6a-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32c6a-108">ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="32c6a-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="32c6a-109">Esto es útil para escenarios que llegan los fragmentos de datos con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="32c6a-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="32c6a-110">Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="32c6a-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="32c6a-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32c6a-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="32c6a-112">Configurar un concentrador para streaming</span><span class="sxs-lookup"><span data-stu-id="32c6a-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32c6a-113">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-113">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="32c6a-114">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una <xref:System.Threading.Channels.ChannelReader%601> o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="32c6a-115">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="32c6a-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="32c6a-116">Métodos de concentrador de streaming puede devolver `IAsyncEnumerable<T>` además `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="32c6a-117">La manera más sencilla para devolver `IAsyncEnumerable<T>` está haciendo que el método de concentrador un método de iterador asincrónico como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="32c6a-118">Métodos de iterador de async de concentrador pueden aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="32c6a-119">Métodos de iterador Async evitar problemas comunes con los canales, como no devolver el `ChannelReader` lo suficientemente pronto o sale del método sin completar la <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="32c6a-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="32c6a-120">El ejemplo siguiente muestra los conceptos básicos de transmisión de datos al cliente utilizando los canales.</span><span class="sxs-lookup"><span data-stu-id="32c6a-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="32c6a-121">Cada vez que se escribe un objeto en el <xref:System.Threading.Channels.ChannelWriter%601>, el objeto inmediatamente se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="32c6a-122">Al final, el `ChannelWriter` completada para indicar al cliente la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="32c6a-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="32c6a-123">Escribir en el `ChannelWriter<T>` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="32c6a-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="32c6a-124">Las demás invocaciones de concentrador se bloquean hasta que un `ChannelReader` se devuelve.</span><span class="sxs-lookup"><span data-stu-id="32c6a-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="32c6a-125">Encapsular la lógica en un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="32c6a-126">Completar la `Channel` en el `catch` como fuera el `catch` para asegurarse de que el centro de invocación del método se completó correctamente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="32c6a-127">Métodos de concentrador de servidor a cliente streaming pueden aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="32c6a-128">Use este token para detener la operación del servidor y libere cualquier recurso si el cliente se desconecta antes del final de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="32c6a-129">Cliente a servidor de transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="32c6a-129">Client-to-server streaming</span></span>

<span data-ttu-id="32c6a-130">Un método de concentrador se convierte automáticamente en un método de concentrador de cliente a servidor streaming cuando acepta uno o varios <xref:System.Threading.Channels.ChannelReader`1>s.</span><span class="sxs-lookup"><span data-stu-id="32c6a-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more <xref:System.Threading.Channels.ChannelReader`1>s.</span></span> <span data-ttu-id="32c6a-131">El ejemplo siguiente muestra los conceptos básicos de lectura de transmisión por secuencias datos enviados desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="32c6a-132">Cada vez que el cliente se escribe en el <xref:System.Threading.Channels.ChannelWriter`1>, los datos se escriben en el `ChannelReader` en el servidor que esté leyendo el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c6a-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter`1>, the data is written into the `ChannelReader` on the server that the hub method is reading from.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="32c6a-133">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="32c6a-133">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="32c6a-134">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="32c6a-134">Server-to-client streaming</span></span>

<span data-ttu-id="32c6a-135">El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias del servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-135">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="32c6a-136">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-136">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="32c6a-137">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="32c6a-137">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="32c6a-138">Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-138">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="32c6a-139">Cliente a servidor de transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="32c6a-139">Client-to-server streaming</span></span>

<span data-ttu-id="32c6a-140">Para invocar un método de concentrador de cliente a servidor streaming desde el cliente. NET, cree un `Channel` y pase el `ChannelReader` como argumento a `SendAsync`, `InvokeAsync`, o `StreamAsChannelAsync`, según el método de concentrador que se invoca.</span><span class="sxs-lookup"><span data-stu-id="32c6a-140">To invoke a client-to-server streaming hub method from the .NET client, create a `Channel` and pass the `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="32c6a-141">Cada vez que se escriben datos en el `ChannelWriter`, el método de concentrador en el servidor recibe un nuevo elemento con los datos del cliente.</span><span class="sxs-lookup"><span data-stu-id="32c6a-141">Whenever data is written to the `ChannelWriter`, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="32c6a-142">Para finalizar la secuencia, complete el canal con `channel.Writer.Complete()`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-142">To end the stream, complete the channel with `channel.Writer.Complete()`.</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="32c6a-143">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="32c6a-143">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="32c6a-144">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="32c6a-144">Server-to-client streaming</span></span>

<span data-ttu-id="32c6a-145">Los clientes de JavaScript llamar a métodos de transmisión por secuencias de servidor a cliente en los centros con `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-145">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="32c6a-146">El `stream` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="32c6a-146">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="32c6a-147">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c6a-147">The name of the hub method.</span></span> <span data-ttu-id="32c6a-148">En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-148">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="32c6a-149">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c6a-149">Arguments defined in the hub method.</span></span> <span data-ttu-id="32c6a-150">En el ejemplo siguiente, los argumentos son un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-150">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="32c6a-151">`connection.stream` Devuelve un `IStreamResult`, que contiene un `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="32c6a-151">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="32c6a-152">Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones desde el `stream` invocación.</span><span class="sxs-lookup"><span data-stu-id="32c6a-152">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="32c6a-153">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="32c6a-153">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="32c6a-154">Llamar a este método produce la cancelación de la `CancellationToken` parámetro del método de concentrador, si se proporciona uno.</span><span class="sxs-lookup"><span data-stu-id="32c6a-154">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="32c6a-155">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="32c6a-155">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="32c6a-156">Cliente a servidor de transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="32c6a-156">Client-to-server streaming</span></span>

<span data-ttu-id="32c6a-157">Los clientes JavaScript llamar a métodos de transmisión por secuencias de cliente a servidor en los centros de pasando un `Subject` como argumento a `send`, `invoke`, o `stream`, según el método de concentrador que se invoca.</span><span class="sxs-lookup"><span data-stu-id="32c6a-157">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="32c6a-158">El `Subject` es una clase que es similar a un `Subject`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-158">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="32c6a-159">Por ejemplo en RxJS, puede usar el [asunto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) clase a partir de esa biblioteca.</span><span class="sxs-lookup"><span data-stu-id="32c6a-159">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="32c6a-160">Una llamada a `subject.next(item)` con un elemento, escribe el elemento en la secuencia y el método de concentrador recibe el elemento en el servidor.</span><span class="sxs-lookup"><span data-stu-id="32c6a-160">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="32c6a-161">Para finalizar el flujo, llame a `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-161">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="32c6a-162">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="32c6a-162">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="32c6a-163">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="32c6a-163">Server-to-client streaming</span></span>

<span data-ttu-id="32c6a-164">El cliente de SignalR Java usa el `stream` método para invocar métodos de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="32c6a-164">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="32c6a-165">`stream` acepta tres o más argumentos:</span><span class="sxs-lookup"><span data-stu-id="32c6a-165">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="32c6a-166">El tipo esperado de los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-166">The expected type of the stream items.</span></span>
* <span data-ttu-id="32c6a-167">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c6a-167">The name of the hub method.</span></span>
* <span data-ttu-id="32c6a-168">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c6a-168">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="32c6a-169">El `stream` método `HubConnection` devuelve un objeto Observable del tipo de elemento de secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c6a-169">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="32c6a-170">El tipo Observable `subscribe` método es donde `onNext`, `onError` y `onCompleted` se definen los controladores.</span><span class="sxs-lookup"><span data-stu-id="32c6a-170">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="32c6a-171">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="32c6a-171">Additional resources</span></span>

* [<span data-ttu-id="32c6a-172">Concentradores</span><span class="sxs-lookup"><span data-stu-id="32c6a-172">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="32c6a-173">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="32c6a-173">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="32c6a-174">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="32c6a-174">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="32c6a-175">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="32c6a-175">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
