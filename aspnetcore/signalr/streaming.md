---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: e0d201a7ffebbbe387a874c6d788994faa2be7a5
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098810"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usar la transmisión por secuencias en ASP.NET Core SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor. Esto es útil para escenarios de fragmentos de datos de procedencia con el tiempo. Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Configurar el centro

Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>` o `Task<ChannelReader<T>>`. A continuación es un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente. Cada vez que se escribe un objeto en el `ChannelReader` ese objeto inmediatamente se envía al cliente. Al final, el `ChannelReader` completada para indicar al cliente la secuencia está cerrada.

> [!NOTE]
> * Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible. Las demás invocaciones de concentrador se bloqueará hasta que un `ChannelReader` se devuelve.
> * Encapsular la lógica en un `try ... catch` y complete el `Channel` en la captura y fuera de la instrucción catch para asegurarse de que el centro de invocación del método se completó correctamente.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

En ASP.NET Core 2.2 o posterior, la transmisión por secuencias los métodos de concentrador puede aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia. Use este token para detener la operación del servidor y libere cualquier recurso si el cliente se desconecta antes del final de la secuencia.

::: moniker-end

## <a name="net-client"></a>Cliente .NET

El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente. Para leer datos, un patrón común es para recorrer en bucle `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles. El bucle finalizará cuando el servidor ha cerrado la secuencia o el token de cancelación pasado a `StreamAsChannelAsync` se cancela.

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
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

## <a name="javascript-client"></a>Cliente de JavaScript

Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`. El `stream` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.
* Argumentos definidos en el método de concentrador. En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.

`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método. Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones, la `stream` invocación.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método. Llamar a este método hará que el `CancellationToken` parámetro del método de concentrador (si se proporciona uno) se cancele.

::: moniker-end

## <a name="related-resources"></a>Recursos relacionados

* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
