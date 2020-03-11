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
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>Usar streaming en ASP.NET Core SignalR

Por [Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR admite el streaming desde el cliente al servidor y desde el servidor al cliente. Esto resulta útil para escenarios en los que llegan fragmentos de datos a lo largo del tiempo. Al transmitir por secuencias, cada fragmento se envía al cliente o al servidor en cuanto está disponible, en lugar de esperar a que todos los datos estén disponibles.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR admite el streaming de valores devueltos de métodos de servidor. Esto resulta útil para escenarios en los que llegan fragmentos de datos a lo largo del tiempo. Cuando un valor devuelto se transmite al cliente, cada fragmento se envía al cliente en cuanto está disponible, en lugar de esperar a que todos los datos estén disponibles.

::: moniker-end

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Configuración de un centro para la transmisión por secuencias

::: moniker range=">= aspnetcore-3.0"

Un método de concentrador se convierte automáticamente en un método de concentrador de streaming cuando devuelve <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`o `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un método de concentrador se convierte automáticamente en un método de concentrador de streaming cuando devuelve una <xref:System.Threading.Channels.ChannelReader%601> o un `Task<ChannelReader<T>>`.

::: moniker-end

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

::: moniker range=">= aspnetcore-3.0"

Los métodos de streaming Hub pueden devolver `IAsyncEnumerable<T>` además de `ChannelReader<T>`. La manera más sencilla de devolver `IAsyncEnumerable<T>` es hacer que el método de concentrador sea un método de iterador asincrónico como se muestra en el ejemplo siguiente. Los métodos de iterador Async de concentrador pueden aceptar un parámetro `CancellationToken` que se desencadena cuando el cliente cancela la suscripción de la secuencia. Los métodos de iterador Async evitan problemas comunes con los canales, como no devolver el `ChannelReader` suficientemente pronto o salir del método sin completar el <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

En el ejemplo siguiente se muestran los aspectos básicos de la transmisión de datos al cliente mediante canales. Cada vez que un objeto se escribe en el <xref:System.Threading.Channels.ChannelWriter%601>, el objeto se envía inmediatamente al cliente. Al final, el `ChannelWriter` se completa para indicar al cliente que la secuencia está cerrada.

> [!NOTE]
> Escriba en el `ChannelWriter<T>` en un subproceso en segundo plano y devuelva el `ChannelReader` lo antes posible. Otras invocaciones del concentrador se bloquean hasta que se devuelve un `ChannelReader`.
>
> Ajusta la lógica en un `try ... catch`. Complete el `Channel` en el `catch` y fuera de la `catch` para asegurarse de que la invocación del método de concentrador se ha completado correctamente.

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

Los métodos del concentrador de streaming de servidor a cliente pueden aceptar un parámetro `CancellationToken` que se desencadena cuando el cliente cancela la suscripción de la secuencia. Use este token para detener la operación del servidor y liberar los recursos si el cliente se desconecta antes del final de la secuencia.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Streaming de cliente a servidor

Un método de concentrador se convierte automáticamente en un método de concentrador de streaming de cliente a servidor cuando acepta uno o más objetos de tipo <xref:System.Threading.Channels.ChannelReader%601> o <xref:System.Collections.Generic.IAsyncEnumerable%601>. En el ejemplo siguiente se muestran los aspectos básicos de la lectura de datos de streaming enviados desde el cliente. Cada vez que el cliente escribe en el <xref:System.Threading.Channels.ChannelWriter%601>, los datos se escriben en el `ChannelReader` en el servidor desde el que el método de concentrador está leyendo.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

A continuación se muestra una versión <xref:System.Collections.Generic.IAsyncEnumerable%601> del método.

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

## <a name="net-client"></a>Cliente .NET

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente


::: moniker range=">= aspnetcore-3.0"

Los métodos `StreamAsync` y `StreamAsChannelAsync` de `HubConnection` se usan para invocar métodos de streaming de servidor a cliente. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsync` o `StreamAsChannelAsync`. El parámetro genérico en `StreamAsync<T>` y `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Un objeto de tipo `IAsyncEnumerable<T>` o `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.

Un ejemplo `StreamAsync` que devuelve `IAsyncEnumerable<int>`:

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

Un ejemplo de `StreamAsChannelAsync` correspondiente que devuelve `ChannelReader<int>`:

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

El método `StreamAsChannelAsync` en `HubConnection` se utiliza para invocar un método de streaming de servidor a cliente. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Una `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.

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

El método `StreamAsChannelAsync` en `HubConnection` se utiliza para invocar un método de streaming de servidor a cliente. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`. El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias. Una `ChannelReader<T>` se devuelve de la invocación de flujo y representa la secuencia en el cliente.

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

### <a name="client-to-server-streaming"></a>Streaming de cliente a servidor

Hay dos maneras de invocar un método de concentrador de streaming de cliente a servidor desde el cliente .NET. Puede pasar un `IAsyncEnumerable<T>` o un `ChannelReader` como un argumento a `SendAsync`, `InvokeAsync`o `StreamAsChannelAsync`, dependiendo del método de concentrador invocado.

Cada vez que los datos se escriben en el objeto `IAsyncEnumerable` o `ChannelWriter`, el método de concentrador del servidor recibe un nuevo elemento con los datos del cliente.

Si se usa un objeto de `IAsyncEnumerable`, la secuencia termina después de que se cierre el método que devuelve los elementos de la secuencia.

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

O bien, si utiliza un `ChannelWriter`, complete el canal con `channel.Writer.Complete()`:

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

Los clientes de JavaScript llaman a los métodos de streaming de servidor a cliente en los concentradores con `connection.stream`. El método `stream` acepta dos argumentos:

* Nombre del método de concentrador. En el ejemplo siguiente, el nombre del método de concentrador es `Counter`.
* Argumentos definidos en el método de concentrador. En el ejemplo siguiente, los argumentos son un recuento del número de elementos de secuencia que se van a recibir y el retraso entre los elementos de la secuencia.

`connection.stream` devuelve un `IStreamResult`, que contiene un método `subscribe`. Pase un `IStreamSubscriber` a `subscribe` y establezca las devoluciones de llamada `next`, `error`y `complete` para recibir notificaciones de la invocación de `stream`.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia del cliente, llame al método `dispose` en el `ISubscription` devuelto desde el método `subscribe`. La llamada a este método provoca la cancelación del parámetro `CancellationToken` del método de concentrador, si se proporciona uno.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Para finalizar la secuencia del cliente, llame al método `dispose` en el `ISubscription` devuelto desde el método `subscribe`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Streaming de cliente a servidor

Los clientes de JavaScript llaman a métodos de streaming de cliente a servidor en los concentradores pasando un `Subject` como argumento a `send`, `invoke`o `stream`, dependiendo del método de concentrador invocado. El `Subject` es una clase que se parece a una `Subject`. Por ejemplo, en RxJS, puede usar la clase de [asunto](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) de la biblioteca.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

La llamada a `subject.next(item)` con un elemento escribe el elemento en la secuencia y el método de concentrador recibe el elemento en el servidor.

Para finalizar la secuencia, llame a `subject.complete()`.

## <a name="java-client"></a>Cliente de Java

### <a name="server-to-client-streaming"></a>Streaming de servidor a cliente

El cliente SignalR Java utiliza el método `stream` para invocar métodos de streaming. `stream` acepta tres o más argumentos:

* Tipo esperado de los elementos de la secuencia.
* Nombre del método de concentrador.
* Argumentos definidos en el método de concentrador.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

El método `stream` en `HubConnection` devuelve un objeto observable del tipo de elemento de secuencia. El método `subscribe` del tipo observable es donde se definen los controladores `onNext`, `onError` y `onCompleted`.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
