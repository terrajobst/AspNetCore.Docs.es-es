---
title: Uso de ASP.NET Core SignalR transmisión por secuencias
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275855"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="3ae1e-102">Uso de ASP.NET Core SignalR transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="3ae1e-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="3ae1e-103">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3ae1e-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="3ae1e-104">Núcleo de ASP.NET SignalR es compatible con transmisión por secuencias valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="3ae1e-105">Esto es útil para escenarios donde fragmentos de datos aparecerá en con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="3ae1e-106">Cuando un valor devuelto se transmite al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de esperar a todos los datos hasta que esté disponible.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="3ae1e-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ae1e-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="3ae1e-108">Configurar el concentrador</span><span class="sxs-lookup"><span data-stu-id="3ae1e-108">Set up the hub</span></span>

<span data-ttu-id="3ae1e-109">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando devuelve un `ChannelReader<T>` o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="3ae1e-110">A continuación se muestra un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="3ae1e-111">Cada vez que un objeto se escribe en el `ChannelReader` ese objeto se envía inmediatamente al cliente.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="3ae1e-112">Al final, el `ChannelReader` se complete para indicar al cliente la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="3ae1e-113">Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva la `ChannelReader` tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="3ae1e-114">Otras las invocaciones del concentrador se bloqueará hasta que un `ChannelReader` se devuelve.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="3ae1e-115">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="3ae1e-115">.NET client</span></span>

<span data-ttu-id="3ae1e-116">El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="3ae1e-117">Pasar el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="3ae1e-118">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="3ae1e-119">Un `ChannelReader<T>` se devuelve desde la invocación de flujo y representa el flujo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="3ae1e-120">Para leer los datos, es un patrón común para recorrer en `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="3ae1e-121">El bucle terminará cuando se ha cerrado la secuencia por el servidor, o el token de cancelación que se pasa a `StreamAsChannelAsync` se cancela.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="3ae1e-122">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3ae1e-122">JavaScript client</span></span>

<span data-ttu-id="3ae1e-123">Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="3ae1e-124">El `stream` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="3ae1e-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="3ae1e-125">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-125">The name of the hub method.</span></span> <span data-ttu-id="3ae1e-126">En el ejemplo siguiente, el nombre de método de concentrador es `Counter`.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="3ae1e-127">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="3ae1e-128">En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="3ae1e-129">`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="3ae1e-130">Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` las devoluciones de llamada para recibir notificaciones de la `stream` invocación.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="3ae1e-131">Para finalizar la secuencia de la llamada del cliente la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="3ae1e-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3ae1e-132">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="3ae1e-132">Related resources</span></span>

* [<span data-ttu-id="3ae1e-133">Concentradores</span><span class="sxs-lookup"><span data-stu-id="3ae1e-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3ae1e-134">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="3ae1e-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3ae1e-135">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="3ae1e-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3ae1e-136">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="3ae1e-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)