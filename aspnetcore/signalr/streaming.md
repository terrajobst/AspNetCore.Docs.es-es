---
title: Uso de ASP.NET Core SignalR transmisión por secuencias
author: rachelappel
description: ''
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/streaming
ms.openlocfilehash: e8b32d5f25ae3bf8555fd2375c0fbb99a6f8bd53
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358447"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Uso de ASP.NET Core SignalR transmisión por secuencias

Por [Brennan Conroy](https://github.com/BrennanConroy)

Núcleo de ASP.NET SignalR es compatible con transmisión por secuencias valores devueltos de métodos de servidor. Esto es útil para escenarios donde fragmentos de datos aparecerá en con el tiempo. Cuando un valor devuelto se transmite al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de esperar a todos los datos hasta que esté disponible.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurar el concentrador

Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando devuelve un `ChannelReader<T>` o `Task<ChannelReader<T>>`. A continuación se muestra un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente. Cada vez que un objeto se escribe en el `ChannelReader` ese objeto se envía inmediatamente al cliente. Al final, el `ChannelReader` se complete para indicar al cliente la secuencia está cerrada.

> [!NOTE]
> Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva la `ChannelReader` tan pronto como sea posible. Otras las invocaciones del concentrador se bloqueará hasta que un `ChannelReader` se devuelve.

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a>Cliente .NET

El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias. Pasar el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Un `ChannelReader<T>` se devuelve desde la invocación de flujo y representa el flujo en el cliente. Para leer los datos, es un patrón común para recorrer en `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles. El bucle terminará cuando se ha cerrado la secuencia por el servidor, o el token de cancelación que se pasa a `StreamAsChannelAsync` se cancela.

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

* El nombre del método de concentrador. En el ejemplo siguiente, el nombre de método de concentrador es `Counter`.
* Argumentos definidos en el método de concentrador. En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.

`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método. Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` las devoluciones de llamada para recibir notificaciones de la `stream` invocación.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia de la llamada del cliente la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.

## <a name="related-resources"></a>Recursos relacionados

* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)