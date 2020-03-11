---
title: Usar streaming en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información acerca de cómo transmitir datos entre el cliente y el servidor.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 21dd8180fe168f81ed68b01f02b81a6264d6e5a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654977"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="e99a9-103">Usar streaming en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e99a9-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e99a9-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="e99a9-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e99a9-105">ASP.NET Core SignalR admite el streaming desde el cliente al servidor y desde el servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="e99a9-106">Esto resulta útil para escenarios en los que llegan fragmentos de datos a lo largo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="e99a9-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="e99a9-107">Al transmitir por secuencias, cada fragmento se envía al cliente o al servidor en cuanto está disponible, en lugar de esperar a que todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="e99a9-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e99a9-108">ASP.NET Core SignalR admite el streaming de valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="e99a9-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="e99a9-109">Esto resulta útil para escenarios en los que llegan fragmentos de datos a lo largo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="e99a9-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="e99a9-110">Cuando un valor devuelto se transmite al cliente, cada fragmento se envía al cliente en cuanto está disponible, en lugar de esperar a que todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="e99a9-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="e99a9-111">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e99a9-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="e99a9-112">Configuración de un centro para la transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="e99a9-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e99a9-113">Un método de concentrador se convierte automáticamente en un método de concentrador de streaming cuando devuelve <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e99a9-114">Un método de concentrador se convierte automáticamente en un método de concentrador de streaming cuando devuelve una <xref:System.Threading.Channels.ChannelReader%601> o un `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="e99a9-115">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="e99a9-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e99a9-116">Los métodos de streaming Hub pueden devolver `IAsyncEnumerable<T>` además de `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="e99a9-117">La manera más sencilla de devolver `IAsyncEnumerable<T>` es hacer que el método de concentrador sea un método de iterador asincrónico como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="e99a9-118">Los métodos de iterador Async de concentrador pueden aceptar un parámetro `CancellationToken` que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="e99a9-119">Los métodos de iterador Async evitan problemas comunes con los canales, como no devolver el `ChannelReader` suficientemente pronto o salir del método sin completar el <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="e99a9-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="e99a9-120">En el ejemplo siguiente se muestran los aspectos básicos de la transmisión de datos al cliente mediante canales.</span><span class="sxs-lookup"><span data-stu-id="e99a9-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="e99a9-121">Cada vez que un objeto se escribe en el <xref:System.Threading.Channels.ChannelWriter%601>, el objeto se envía inmediatamente al cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="e99a9-122">Al final, el `ChannelWriter` se completa para indicar al cliente que la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="e99a9-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="e99a9-123">Escriba en el `ChannelWriter<T>` en un subproceso en segundo plano y devuelva el `ChannelReader` lo antes posible.</span><span class="sxs-lookup"><span data-stu-id="e99a9-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="e99a9-124">Otras invocaciones del concentrador se bloquean hasta que se devuelve un `ChannelReader`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="e99a9-125">Ajusta la lógica en un `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="e99a9-126">Complete el `Channel` en el `catch` y fuera de la `catch` para asegurarse de que la invocación del método de concentrador se ha completado correctamente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="e99a9-127">Los métodos del concentrador de streaming de servidor a cliente pueden aceptar un parámetro `CancellationToken` que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="e99a9-128">Use este token para detener la operación del servidor y liberar los recursos si el cliente se desconecta antes del final de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="e99a9-129">Streaming de cliente a servidor</span><span class="sxs-lookup"><span data-stu-id="e99a9-129">Client-to-server streaming</span></span>

<span data-ttu-id="e99a9-130">Un método de concentrador se convierte automáticamente en un método de concentrador de streaming de cliente a servidor cuando acepta uno o más objetos de tipo <xref:System.Threading.Channels.ChannelReader%601> o <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="e99a9-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="e99a9-131">En el ejemplo siguiente se muestran los aspectos básicos de la lectura de datos de streaming enviados desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="e99a9-132">Cada vez que el cliente escribe en el <xref:System.Threading.Channels.ChannelWriter%601>, los datos se escriben en el `ChannelReader` en el servidor desde el que el método de concentrador está leyendo.</span><span class="sxs-lookup"><span data-stu-id="e99a9-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="e99a9-133">A continuación se muestra una versión <xref:System.Collections.Generic.IAsyncEnumerable%601> del método.</span><span class="sxs-lookup"><span data-stu-id="e99a9-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="e99a9-134">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="e99a9-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e99a9-135">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="e99a9-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e99a9-136">Los métodos `StreamAsync` y `StreamAsChannelAsync` de `HubConnection` se usan para invocar métodos de streaming de servidor a cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="e99a9-137">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsync` o `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e99a9-138">El parámetro genérico en `StreamAsync<T>` y `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="e99a9-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e99a9-139">Un objeto de tipo `IAsyncEnumerable<T>` o `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="e99a9-140">Un ejemplo `StreamAsync` que devuelve `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="e99a9-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

<span data-ttu-id="e99a9-141">Un ejemplo de `StreamAsChannelAsync` correspondiente que devuelve `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="e99a9-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e99a9-142">El método `StreamAsChannelAsync` en `HubConnection` se utiliza para invocar un método de streaming de servidor a cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="e99a9-143">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e99a9-144">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="e99a9-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e99a9-145">Una `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="e99a9-146">El método `StreamAsChannelAsync` en `HubConnection` se utiliza para invocar un método de streaming de servidor a cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="e99a9-147">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e99a9-148">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="e99a9-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e99a9-149">Una `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="e99a9-150">Streaming de cliente a servidor</span><span class="sxs-lookup"><span data-stu-id="e99a9-150">Client-to-server streaming</span></span>

<span data-ttu-id="e99a9-151">Hay dos maneras de invocar un método de concentrador de streaming de cliente a servidor desde el cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="e99a9-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="e99a9-152">Puede pasar un `IAsyncEnumerable<T>` o un `ChannelReader` como un argumento a `SendAsync`, `InvokeAsync`o `StreamAsChannelAsync`, dependiendo del método de concentrador invocado.</span><span class="sxs-lookup"><span data-stu-id="e99a9-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="e99a9-153">Cada vez que los datos se escriben en el objeto `IAsyncEnumerable` o `ChannelWriter`, el método de concentrador del servidor recibe un nuevo elemento con los datos del cliente.</span><span class="sxs-lookup"><span data-stu-id="e99a9-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="e99a9-154">Si se usa un objeto de `IAsyncEnumerable`, la secuencia termina después de que se cierre el método que devuelve los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="e99a9-155">O bien, si utiliza un `ChannelWriter`, complete el canal con `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="e99a9-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="e99a9-156">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e99a9-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e99a9-157">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="e99a9-157">Server-to-client streaming</span></span>

<span data-ttu-id="e99a9-158">Los clientes de JavaScript llaman a los métodos de streaming de servidor a cliente en los concentradores con `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="e99a9-159">El método `stream` acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="e99a9-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="e99a9-160">Nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="e99a9-160">The name of the hub method.</span></span> <span data-ttu-id="e99a9-161">En el ejemplo siguiente, el nombre del método de concentrador es `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="e99a9-162">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="e99a9-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="e99a9-163">En el ejemplo siguiente, los argumentos son un recuento del número de elementos de secuencia que se van a recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="e99a9-164">`connection.stream` devuelve un `IStreamResult`, que contiene un método `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="e99a9-165">Pase un `IStreamSubscriber` a `subscribe` y establezca las devoluciones de llamada `next`, `error`y `complete` para recibir notificaciones de la invocación de `stream`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e99a9-166">Para finalizar la secuencia del cliente, llame al método `dispose` en el `ISubscription` devuelto desde el método `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="e99a9-167">La llamada a este método provoca la cancelación del parámetro `CancellationToken` del método de concentrador, si se proporciona uno.</span><span class="sxs-lookup"><span data-stu-id="e99a9-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e99a9-168">Para finalizar la secuencia del cliente, llame al método `dispose` en el `ISubscription` devuelto desde el método `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="e99a9-169">Streaming de cliente a servidor</span><span class="sxs-lookup"><span data-stu-id="e99a9-169">Client-to-server streaming</span></span>

<span data-ttu-id="e99a9-170">Los clientes de JavaScript llaman a métodos de streaming de cliente a servidor en los concentradores pasando un `Subject` como argumento a `send`, `invoke`o `stream`, dependiendo del método de concentrador invocado.</span><span class="sxs-lookup"><span data-stu-id="e99a9-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="e99a9-171">El `Subject` es una clase que se parece a una `Subject`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="e99a9-172">Por ejemplo, en RxJS, puede usar la clase de [asunto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="e99a9-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="e99a9-173">La llamada a `subject.next(item)` con un elemento escribe el elemento en la secuencia y el método de concentrador recibe el elemento en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e99a9-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="e99a9-174">Para finalizar la secuencia, llame a `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="e99a9-175">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="e99a9-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e99a9-176">Streaming de servidor a cliente</span><span class="sxs-lookup"><span data-stu-id="e99a9-176">Server-to-client streaming</span></span>

<span data-ttu-id="e99a9-177">El cliente SignalR Java utiliza el método `stream` para invocar métodos de streaming.</span><span class="sxs-lookup"><span data-stu-id="e99a9-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="e99a9-178">`stream` acepta tres o más argumentos:</span><span class="sxs-lookup"><span data-stu-id="e99a9-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="e99a9-179">Tipo esperado de los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="e99a9-180">Nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="e99a9-180">The name of the hub method.</span></span>
* <span data-ttu-id="e99a9-181">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="e99a9-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="e99a9-182">El método `stream` en `HubConnection` devuelve un objeto observable del tipo de elemento de secuencia.</span><span class="sxs-lookup"><span data-stu-id="e99a9-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="e99a9-183">El método `subscribe` del tipo observable es donde se definen los controladores `onNext`, `onError` y `onCompleted`.</span><span class="sxs-lookup"><span data-stu-id="e99a9-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e99a9-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e99a9-184">Additional resources</span></span>

* [<span data-ttu-id="e99a9-185">Concentradores</span><span class="sxs-lookup"><span data-stu-id="e99a9-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e99a9-186">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="e99a9-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e99a9-187">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e99a9-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e99a9-188">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="e99a9-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
