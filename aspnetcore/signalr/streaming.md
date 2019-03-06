---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo devolver secuencias de valores de los métodos de concentrador de servidor y consumir los flujos con los clientes de .NET y JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345592"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="60d89-103">Usar la transmisión por secuencias en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="60d89-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="60d89-104">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="60d89-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="60d89-105">ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="60d89-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="60d89-106">Esto es útil para escenarios de fragmentos de datos de procedencia con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="60d89-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="60d89-107">Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="60d89-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="60d89-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60d89-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="60d89-109">Configurar el centro</span><span class="sxs-lookup"><span data-stu-id="60d89-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="60d89-110">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="60d89-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="60d89-111">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>` o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="60d89-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="60d89-112">En ASP.NET Core 3.0 o posterior, puede devolver la transmisión por secuencias los métodos de concentrador `IAsyncEnumerable<T>` además `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="60d89-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="60d89-113">La manera más sencilla para devolver `IAsyncEnumerable<T>` está haciendo que el método de concentrador un método de iterador asincrónico como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="60d89-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="60d89-114">Métodos de iterador de async de concentrador pueden aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="60d89-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="60d89-115">Métodos de iterador asincrónicos muy fácil evitar problemas comunes con los canales como no devolver el `ChannelReader` lo suficientemente pronto o sale del método sin completar la `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="60d89-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="60d89-116">El ejemplo siguiente muestra los conceptos básicos de transmisión de datos al cliente utilizando los canales.</span><span class="sxs-lookup"><span data-stu-id="60d89-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="60d89-117">Cada vez que se escribe un objeto en el `ChannelWriter` ese objeto inmediatamente se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="60d89-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="60d89-118">Al final, el `ChannelWriter` completada para indicar al cliente la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="60d89-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="60d89-119">Escribir en el `ChannelWriter` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="60d89-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="60d89-120">Las demás invocaciones de concentrador se bloqueará hasta que un `ChannelReader` se devuelve.</span><span class="sxs-lookup"><span data-stu-id="60d89-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="60d89-121">Encapsular la lógica en un `try ... catch` y complete el `Channel` en la captura y fuera de la instrucción catch para asegurarse de que el centro de invocación del método se completó correctamente.</span><span class="sxs-lookup"><span data-stu-id="60d89-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="60d89-122">En ASP.NET Core 2.2 o posterior, la transmisión por secuencias los métodos de concentrador puede aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="60d89-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="60d89-123">Use este token para detener la operación del servidor y libere cualquier recurso si el cliente se desconecta antes del final de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="60d89-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="60d89-124">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="60d89-124">.NET client</span></span>

<span data-ttu-id="60d89-125">El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="60d89-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="60d89-126">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="60d89-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="60d89-127">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="60d89-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="60d89-128">Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="60d89-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="60d89-129">Para leer datos, un patrón común es para recorrer en bucle `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles.</span><span class="sxs-lookup"><span data-stu-id="60d89-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="60d89-130">El bucle finalizará cuando el servidor ha cerrado la secuencia o el token de cancelación pasado a `StreamAsChannelAsync` se cancela.</span><span class="sxs-lookup"><span data-stu-id="60d89-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="60d89-131">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="60d89-131">JavaScript client</span></span>

<span data-ttu-id="60d89-132">Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="60d89-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="60d89-133">El `stream` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="60d89-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="60d89-134">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="60d89-134">The name of the hub method.</span></span> <span data-ttu-id="60d89-135">En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.</span><span class="sxs-lookup"><span data-stu-id="60d89-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="60d89-136">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="60d89-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="60d89-137">En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="60d89-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="60d89-138">`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="60d89-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="60d89-139">Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones, la `stream` invocación.</span><span class="sxs-lookup"><span data-stu-id="60d89-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="60d89-140">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="60d89-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="60d89-141">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="60d89-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="60d89-142">Llamar a este método hará que el `CancellationToken` parámetro del método de concentrador (si se proporciona uno) se cancele.</span><span class="sxs-lookup"><span data-stu-id="60d89-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a><span data-ttu-id="60d89-143">Cliente de Java</span><span class="sxs-lookup"><span data-stu-id="60d89-143">Java client</span></span>
<span data-ttu-id="60d89-144">El cliente de SignalR Java usa el `stream` método para invocar métodos de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="60d89-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="60d89-145">Acepta tres o más argumentos:</span><span class="sxs-lookup"><span data-stu-id="60d89-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="60d89-146">El tipo esperado de los elementos de flujo</span><span class="sxs-lookup"><span data-stu-id="60d89-146">The expected type of the stream items</span></span> 
* <span data-ttu-id="60d89-147">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="60d89-147">The name of the hub method.</span></span>
* <span data-ttu-id="60d89-148">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="60d89-148">Arguments defined in the hub method.</span></span> 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
<span data-ttu-id="60d89-149">El `stream` método `HubConnection` devuelve un objeto Observable del tipo de elemento de secuencia.</span><span class="sxs-lookup"><span data-stu-id="60d89-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="60d89-150">El tipo Observable `subscribe` método es donde se definen los `onNext`, `onError` y `onCompleted` controladores.</span><span class="sxs-lookup"><span data-stu-id="60d89-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="60d89-151">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="60d89-151">Related resources</span></span>

* [<span data-ttu-id="60d89-152">Concentradores</span><span class="sxs-lookup"><span data-stu-id="60d89-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="60d89-153">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="60d89-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="60d89-154">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="60d89-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="60d89-155">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="60d89-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
