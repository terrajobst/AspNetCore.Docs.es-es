---
title: Cliente de .NET de ASP.NET Core SignalR
author: bradygaster
description: Información sobre el cliente de .NET de ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153124"
---
# <a name="aspnet-core-signalr-net-client"></a>Cliente de .NET de ASP.NET Core SignalR

La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.

## <a name="install-the-signalr-net-client-package"></a>Instale el paquete de cliente .NET de SignalR

El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR. Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`. Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones. Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`. Iniciar la conexión con `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Controlar la pérdida de conexión

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Volver a conectar automáticamente

El <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> puede configurarse para volver a conectar automáticamente con el `WithAutomaticReconnect` método en el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. No conectar automáticamente de forma predeterminada.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Sin parámetros, `WithAutomaticReconnect()` configura el cliente que se debe esperar 0, 2, 10 y 30 segundos respectivamente antes de intentar cada intento de volver a conectar, deteniéndose después de cuatro intentos con error.

Antes de iniciar cualquier intento de volver a conectar el `HubConnection` le transición a la `HubConnectionState.Reconnecting` de estado y se activan los `Reconnecting` eventos.  Esto proporciona una oportunidad para advertir a los usuarios que se ha perdido la conexión y deshabilitar los elementos de interfaz de usuario. Puesta en cola o eliminación de mensajes, pueden iniciar aplicaciones no interactivas.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Si el cliente vuelve a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` realizará la transición a la `Connected` de estado y se activan los `Reconnected` eventos. Esto proporciona una oportunidad para informar a los usuarios la conexión se ha restablecido y quitar de la cola los mensajes en cola.

Puesto que la conexión es completamente nuevo en el servidor, un nuevo `ConnectionId` se proporcionará el `Reconnected` controladores de eventos.

> [!WARNING]
> El `Reconnected` del controlador de eventos `connectionId` parámetro será nulo si el `HubConnection` se configuró para [omitir negociación](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` No configurar el `HubConnection` para volver a intentar errores de inicio inicial, por lo que es necesario controlar manualmente los errores de inicio:

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

Si el cliente no volver a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` le transición a la `Disconnected` de estado y se activan los <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos. Esto proporciona una oportunidad para intentar reiniciar la conexión manualmente o informar a los usuarios que la conexión se ha perdido permanentemente.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `WithAutomaticReconnect` acepta una matriz de números que representa el retraso en milisegundos para esperar antes de iniciar cada intento de volver a conectar.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

El ejemplo anterior se configura el `HubConnection` iniciar intentar reconexiones inmediatamente después de que se pierde la conexión. Esto también es cierto para la configuración predeterminada.

Si se produce un error en el primer intento de volver a conectar, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar de 2 segundos como lo haría en la configuración predeterminada.

Si se produce un error en el segundo intento de volver a conectar, se iniciará al tercer intento de reconexión en 10 segundos, que es nuevo como la configuración predeterminada.

El comportamiento personalizado, a continuación, divergirá nuevo desde el comportamiento predeterminado deteniendo tras la tercera conectarse de nuevo intento falla. En la configuración predeterminada se debía ser uno más volver a conectarse intento en otros 30 segundos.

Si desea tener un mayor control sobre la planeación y el número de automático volver a conectarse, los intentos de `WithAutomaticReconnect` acepta un objeto que implementa el `IRetryPolicy` interfaz, que tiene un solo método denominado `NextRetryDelay`.

`NextRetryDelay` toma un único argumento con el tipo `RetryContext`. El `RetryContext` tiene tres propiedades: `PreviousRetryCount`, `ElapsedTime` y `RetryReason` que son un `long`, un `TimeSpan` y un `Exception` respectivamente. Antes del primer intento de reconexión ambos `PreviousRetryCount` y `ElapsedTime` será igual cero y el `RetryReason` será la excepción que produjo la conexión se perderán. Después de cada intento de reintento con errores, `PreviousRetryCount` se incrementa en uno, `ElapsedTime` se actualizará para reflejar la cantidad de tiempo empleado en volver a conectarse hasta ahora y el `RetryReason` será la excepción que produjo el último intento de volver a conectar a un error.

`NextRetryDelay` debe devolver ya sea un objeto TimeSpan que representa la hora debe esperar antes del siguiente intento de volver a conectar o `null` si el `HubConnection` debe detenerse la reconexión.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

Como alternativa, puede escribir código que se volverá a conectar el cliente manualmente como se muestra en [conectar manualmente](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Volver a conectar manualmente

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Antes de 3.0, el cliente de .NET para SignalR no volver a conectar automáticamente. Debe escribir código que se volverá a conectar al cliente manualmente.

::: moniker-end

Use el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a una pérdida de conexión. Por ejemplo, es posible que desee automatizar la reconexión.

El `Closed` evento requiere un delegado que devuelve un `Task`, lo que permite ejecutar sin usar código asincrónico `async void`. Para cumplir con la firma del delegado en un `Closed` controlador de eventos que se ejecuta de forma sincrónica, devolver `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

La razón principal para el soporte para asincronía es por lo que puede reiniciar la conexión. A partir de una conexión es una acción asincrónica.

En un `Closed` controlador que se reinicia la conexión, considere la posibilidad de esperar a cierto retraso aleatorio evitar la sobrecarga del servidor, como se muestra en el ejemplo siguiente:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

`InvokeAsync` llama a métodos en el concentrador. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`. SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

El `InvokeAsync` método devuelve un `Task` que se completa cuando finaliza el método de servidor. El valor devuelto, si existe, se proporciona como el resultado de la `Task`. Las excepciones producidas por el método en el servidor generan un error `Task`. Use `await` sintaxis que espere a que el método de servidor completar y `try...catch` sintaxis para controlar los errores.

El `SendAsync` método devuelve un `Task` que se completa cuando el mensaje se envió al servidor. Se proporciona ningún valor devuelto, ya que `Task` no espere hasta que finalice el método de servidor. Las excepciones producidas en el cliente al enviar el mensaje generan un error `Task`. Use `await` y `try...catch` sintaxis para controlar errores de envío.

> [!NOTE]
> Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente. Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Registro y control de errores

Controlar los errores con una instrucción try-catch. Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Recursos adicionales

* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Documentación sin servidor de Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
