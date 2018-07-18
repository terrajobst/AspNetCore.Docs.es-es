---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095267"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="fb5b7-102">Usar la transmisión por secuencias en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="fb5b7-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="fb5b7-103">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="fb5b7-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="fb5b7-104">ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="fb5b7-105">Esto es útil para escenarios de fragmentos de datos de procedencia con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="fb5b7-106">Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="fb5b7-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb5b7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="fb5b7-108">Configurar el centro</span><span class="sxs-lookup"><span data-stu-id="fb5b7-108">Set up the hub</span></span>

<span data-ttu-id="fb5b7-109">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>` o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="fb5b7-110">A continuación es un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="fb5b7-111">Cada vez que se escribe un objeto en el `ChannelReader` ese objeto inmediatamente se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="fb5b7-112">Al final, el `ChannelReader` completada para indicar al cliente la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="fb5b7-113">Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="fb5b7-114">Las demás invocaciones de concentrador se bloqueará hasta que un `ChannelReader` se devuelve.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="fb5b7-115">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="fb5b7-115">.NET client</span></span>

<span data-ttu-id="fb5b7-116">El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="fb5b7-117">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="fb5b7-118">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="fb5b7-119">Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="fb5b7-120">Para leer datos, un patrón común es para recorrer en bucle `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="fb5b7-121">El bucle finalizará cuando el servidor ha cerrado la secuencia o el token de cancelación pasado a `StreamAsChannelAsync` se cancela.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a><span data-ttu-id="fb5b7-122">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fb5b7-122">JavaScript client</span></span>

<span data-ttu-id="fb5b7-123">Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="fb5b7-124">El `stream` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="fb5b7-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="fb5b7-125">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-125">The name of the hub method.</span></span> <span data-ttu-id="fb5b7-126">En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="fb5b7-127">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="fb5b7-128">En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="fb5b7-129">`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="fb5b7-130">Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones, la `stream` invocación.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="fb5b7-131">Para finalizar la secuencia de la llamada del cliente la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="fb5b7-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="fb5b7-132">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="fb5b7-132">Related resources</span></span>

* [<span data-ttu-id="fb5b7-133">Concentradores</span><span class="sxs-lookup"><span data-stu-id="fb5b7-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="fb5b7-134">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="fb5b7-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="fb5b7-135">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fb5b7-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="fb5b7-136">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="fb5b7-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
