---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo transmitir datos entre el cliente y el servidor.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: d185056d3bdda089eaa46ae9b8e13ab7a4354f93
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165081"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Usar la transmisión por secuencias en ASP.NET Core SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

SignalR de ASP.NET Core admite la transmisión del cliente al servidor y del servidor al cliente. Esto es útil para escenarios que llegan los fragmentos de datos con el tiempo. La transmisión por secuencias, significará que se ha enviado cada fragmento para el cliente o servidor tan pronto como se convierte en disponible, en lugar de esperar todos los datos estén disponibles.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor. Esto es útil para escenarios que llegan los fragmentos de datos con el tiempo. Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.

::: moniker-end

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Configurar un concentrador para streaming

::: moniker range=">= aspnetcore-3.0"

Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, o `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una <xref:System.Threading.Channels.ChannelReader%601> o `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

::: moniker range=">= aspnetcore-3.0"

Métodos de concentrador de streaming puede devolver `IAsyncEnumerable<T>` además `ChannelReader<T>`. La manera más sencilla para devolver `IAsyncEnumerable<T>` está haciendo que el método de concentrador un método de iterador asincrónico como se muestra en el ejemplo siguiente. Métodos de iterador de async de concentrador pueden aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia. Métodos de iterador Async evitar problemas comunes con los canales, como no devolver el `ChannelReader` lo suficientemente pronto o sale del método sin completar la <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

El ejemplo siguiente muestra los conceptos básicos de transmisión de datos al cliente utilizando los canales. Cada vez que se escribe un objeto en el <xref:System.Threading.Channels.ChannelWriter%601>, el objeto inmediatamente se envía al cliente. Al final, el `ChannelWriter` completada para indicar al cliente la secuencia está cerrada.

> [!NOTE]
> Escribir en el `ChannelWriter<T>` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible. Las demás invocaciones de concentrador se bloquean hasta que un `ChannelReader` se devuelve.
>
> Encapsular la lógica en un `try ... catch`. Completar la `Channel` en el `catch` como fuera el `catch` para asegurarse de que el centro de invocación del método se completó correctamente.

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

Métodos de concentrador de servidor a cliente streaming pueden aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia. Use este token para detener la operación del servidor y libere cualquier recurso si el cliente se desconecta antes del final de la secuencia.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Cliente a servidor de transmisión por secuencias

Un método de concentrador se convierte automáticamente en un método de concentrador de cliente a servidor streaming cuando acepta uno o varios <xref:System.Threading.Channels.ChannelReader`1>s. El ejemplo siguiente muestra los conceptos básicos de lectura de transmisión por secuencias datos enviados desde el cliente. Cada vez que el cliente se escribe en el <xref:System.Threading.Channels.ChannelWriter`1>, los datos se escriben en el `ChannelReader` en el servidor que esté leyendo el método de concentrador.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a>Cliente .NET

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias del servidor al cliente. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente.

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

### <a name="client-to-server-streaming"></a>Cliente a servidor de transmisión por secuencias

Para invocar un método de concentrador de cliente a servidor streaming desde el cliente. NET, cree un `Channel` y pase el `ChannelReader` como argumento a `SendAsync`, `InvokeAsync`, o `StreamAsChannelAsync`, según el método de concentrador que se invoca.

Cada vez que se escriben datos en el `ChannelWriter`, el método de concentrador en el servidor recibe un nuevo elemento con los datos del cliente.

Para finalizar la secuencia, complete el canal con `channel.Writer.Complete()`.

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Cliente de JavaScript

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

Los clientes de JavaScript llamar a métodos de transmisión por secuencias de servidor a cliente en los centros con `connection.stream`. El `stream` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.
* Argumentos definidos en el método de concentrador. En el ejemplo siguiente, los argumentos son un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.

`connection.stream` Devuelve un `IStreamResult`, que contiene un `subscribe` método. Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones desde el `stream` invocación.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método. Llamar a este método produce la cancelación de la `CancellationToken` parámetro del método de concentrador, si se proporciona uno.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Cliente a servidor de transmisión por secuencias

Los clientes JavaScript llamar a métodos de transmisión por secuencias de cliente a servidor en los centros de pasando un `Subject` como argumento a `send`, `invoke`, o `stream`, según el método de concentrador que se invoca. El `Subject` es una clase que es similar a un `Subject`. Por ejemplo en RxJS, puede usar el [asunto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) clase a partir de esa biblioteca.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Una llamada a `subject.next(item)` con un elemento, escribe el elemento en la secuencia y el método de concentrador recibe el elemento en el servidor.

Para finalizar el flujo, llame a `subject.complete()`.

## <a name="java-client"></a>Cliente de Java

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

El cliente de SignalR Java usa el `stream` método para invocar métodos de transmisión por secuencias. `stream` acepta tres o más argumentos:

* El tipo esperado de los elementos de la secuencia.
* El nombre del método de concentrador.
* Argumentos definidos en el método de concentrador.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

El `stream` método `HubConnection` devuelve un objeto Observable del tipo de elemento de secuencia. El tipo Observable `subscribe` método es donde `onNext`, `onError` y `onCompleted` se definen los controladores.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
