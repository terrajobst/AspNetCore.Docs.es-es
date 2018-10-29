---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 70f12999b7f4230147b9ea43f6f7730b0816c43a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206393"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usar la transmisión por secuencias en ASP.NET Core SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor. Esto es útil para escenarios de fragmentos de datos de procedencia con el tiempo. Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurar el centro

Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>` o `Task<ChannelReader<T>>`. A continuación es un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente. Cada vez que se escribe un objeto en el `ChannelReader` ese objeto inmediatamente se envía al cliente. Al final, el `ChannelReader` completada para indicar al cliente la secuencia está cerrada.

> [!NOTE]
> Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible. Las demás invocaciones de concentrador se bloqueará hasta que un `ChannelReader` se devuelve.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Cliente .NET

El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente. Para leer datos, un patrón común es para recorrer en bucle `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles. El bucle finalizará cuando el servidor ha cerrado la secuencia o el token de cancelación pasado a `StreamAsChannelAsync` se cancela.

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

## <a name="javascript-client"></a>Cliente de JavaScript

Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`. El `stream` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.
* Argumentos definidos en el método de concentrador. En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.

`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método. Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones, la `stream` invocación.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.

## <a name="related-resources"></a>Recursos relacionados

* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
