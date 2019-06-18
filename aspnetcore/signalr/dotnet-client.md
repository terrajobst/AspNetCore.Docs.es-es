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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="dbc57-103">Cliente de .NET de ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="dbc57-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="dbc57-104">La biblioteca de cliente de ASP.NET Core SignalR .NET le permite comunicarse con los concentradores de SignalR desde aplicaciones de. NET.</span><span class="sxs-lookup"><span data-stu-id="dbc57-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="dbc57-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dbc57-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dbc57-106">El ejemplo de código en este artículo es una aplicación WPF que usa al cliente de .NET de ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="dbc57-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="dbc57-107">Instale el paquete de cliente .NET de SignalR</span><span class="sxs-lookup"><span data-stu-id="dbc57-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="dbc57-108">El `Microsoft.AspNetCore.SignalR.Client` paquete es necesario para que los clientes de .NET para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="dbc57-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="dbc57-109">Para instalar la biblioteca de cliente, ejecute el siguiente comando en el **Package Manager Console** ventana:</span><span class="sxs-lookup"><span data-stu-id="dbc57-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="dbc57-110">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="dbc57-110">Connect to a hub</span></span>

<span data-ttu-id="dbc57-111">Para establecer una conexión, cree un `HubConnectionBuilder` y llamar a `Build`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="dbc57-112">Durante la compilación de una conexión se pueden configurar la URL del centro, protocolo, tipo de transporte, nivel de registro, los encabezados y otras opciones.</span><span class="sxs-lookup"><span data-stu-id="dbc57-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="dbc57-113">Configurar las opciones necesarias mediante la inserción de cualquiera de los `HubConnectionBuilder` métodos en `Build`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="dbc57-114">Iniciar la conexión con `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="dbc57-115">Controlar la pérdida de conexión</span><span class="sxs-lookup"><span data-stu-id="dbc57-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="dbc57-116">Volver a conectar automáticamente</span><span class="sxs-lookup"><span data-stu-id="dbc57-116">Automatically reconnect</span></span>

<span data-ttu-id="dbc57-117">El <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> puede configurarse para volver a conectar automáticamente con el `WithAutomaticReconnect` método en el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dbc57-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="dbc57-118">No conectar automáticamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dbc57-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="dbc57-119">Sin parámetros, `WithAutomaticReconnect()` configura el cliente que se debe esperar 0, 2, 10 y 30 segundos respectivamente antes de intentar cada intento de volver a conectar, deteniéndose después de cuatro intentos con error.</span><span class="sxs-lookup"><span data-stu-id="dbc57-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="dbc57-120">Antes de iniciar cualquier intento de volver a conectar el `HubConnection` le transición a la `HubConnectionState.Reconnecting` de estado y se activan los `Reconnecting` eventos.</span><span class="sxs-lookup"><span data-stu-id="dbc57-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="dbc57-121">Esto proporciona una oportunidad para advertir a los usuarios que se ha perdido la conexión y deshabilitar los elementos de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="dbc57-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="dbc57-122">Puesta en cola o eliminación de mensajes, pueden iniciar aplicaciones no interactivas.</span><span class="sxs-lookup"><span data-stu-id="dbc57-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="dbc57-123">Si el cliente vuelve a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` realizará la transición a la `Connected` de estado y se activan los `Reconnected` eventos.</span><span class="sxs-lookup"><span data-stu-id="dbc57-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="dbc57-124">Esto proporciona una oportunidad para informar a los usuarios la conexión se ha restablecido y quitar de la cola los mensajes en cola.</span><span class="sxs-lookup"><span data-stu-id="dbc57-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="dbc57-125">Puesto que la conexión es completamente nuevo en el servidor, un nuevo `ConnectionId` se proporcionará el `Reconnected` controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="dbc57-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="dbc57-126">El `Reconnected` del controlador de eventos `connectionId` parámetro será nulo si el `HubConnection` se configuró para [omitir negociación](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="dbc57-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="dbc57-127">`WithAutomaticReconnect()` No configurar el `HubConnection` para volver a intentar errores de inicio inicial, por lo que es necesario controlar manualmente los errores de inicio:</span><span class="sxs-lookup"><span data-stu-id="dbc57-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="dbc57-128">Si el cliente no volver a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` le transición a la `Disconnected` de estado y se activan los <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos.</span><span class="sxs-lookup"><span data-stu-id="dbc57-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="dbc57-129">Esto proporciona una oportunidad para intentar reiniciar la conexión manualmente o informar a los usuarios que la conexión se ha perdido permanentemente.</span><span class="sxs-lookup"><span data-stu-id="dbc57-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="dbc57-130">Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `WithAutomaticReconnect` acepta una matriz de números que representa el retraso en milisegundos para esperar antes de iniciar cada intento de volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="dbc57-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="dbc57-131">El ejemplo anterior se configura el `HubConnection` iniciar intentar reconexiones inmediatamente después de que se pierde la conexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="dbc57-132">Esto también es cierto para la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dbc57-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="dbc57-133">Si se produce un error en el primer intento de volver a conectar, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar de 2 segundos como lo haría en la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dbc57-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="dbc57-134">Si se produce un error en el segundo intento de volver a conectar, se iniciará al tercer intento de reconexión en 10 segundos, que es nuevo como la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dbc57-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="dbc57-135">El comportamiento personalizado, a continuación, divergirá nuevo desde el comportamiento predeterminado deteniendo tras la tercera conectarse de nuevo intento falla.</span><span class="sxs-lookup"><span data-stu-id="dbc57-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="dbc57-136">En la configuración predeterminada se debía ser uno más volver a conectarse intento en otros 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="dbc57-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="dbc57-137">Si desea tener un mayor control sobre la planeación y el número de automático volver a conectarse, los intentos de `WithAutomaticReconnect` acepta un objeto que implementa el `IRetryPolicy` interfaz, que tiene un solo método denominado `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="dbc57-138">`NextRetryDelay` toma un único argumento con el tipo `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="dbc57-139">El `RetryContext` tiene tres propiedades: `PreviousRetryCount`, `ElapsedTime` y `RetryReason` que son un `long`, un `TimeSpan` y un `Exception` respectivamente.</span><span class="sxs-lookup"><span data-stu-id="dbc57-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="dbc57-140">Antes del primer intento de reconexión ambos `PreviousRetryCount` y `ElapsedTime` será igual cero y el `RetryReason` será la excepción que produjo la conexión se perderán.</span><span class="sxs-lookup"><span data-stu-id="dbc57-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="dbc57-141">Después de cada intento de reintento con errores, `PreviousRetryCount` se incrementa en uno, `ElapsedTime` se actualizará para reflejar la cantidad de tiempo empleado en volver a conectarse hasta ahora y el `RetryReason` será la excepción que produjo el último intento de volver a conectar a un error.</span><span class="sxs-lookup"><span data-stu-id="dbc57-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="dbc57-142">`NextRetryDelay` debe devolver ya sea un objeto TimeSpan que representa la hora debe esperar antes del siguiente intento de volver a conectar o `null` si el `HubConnection` debe detenerse la reconexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="dbc57-143">Como alternativa, puede escribir código que se volverá a conectar el cliente manualmente como se muestra en [conectar manualmente](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="dbc57-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="dbc57-144">Volver a conectar manualmente</span><span class="sxs-lookup"><span data-stu-id="dbc57-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="dbc57-145">Antes de 3.0, el cliente de .NET para SignalR no volver a conectar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="dbc57-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="dbc57-146">Debe escribir código que se volverá a conectar al cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="dbc57-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="dbc57-147">Use el <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> eventos para responder a una pérdida de conexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="dbc57-148">Por ejemplo, es posible que desee automatizar la reconexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="dbc57-149">El `Closed` evento requiere un delegado que devuelve un `Task`, lo que permite ejecutar sin usar código asincrónico `async void`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="dbc57-150">Para cumplir con la firma del delegado en un `Closed` controlador de eventos que se ejecuta de forma sincrónica, devolver `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="dbc57-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="dbc57-151">La razón principal para el soporte para asincronía es por lo que puede reiniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="dbc57-152">A partir de una conexión es una acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="dbc57-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="dbc57-153">En un `Closed` controlador que se reinicia la conexión, considere la posibilidad de esperar a cierto retraso aleatorio evitar la sobrecarga del servidor, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dbc57-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="dbc57-154">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="dbc57-154">Call hub methods from client</span></span>

<span data-ttu-id="dbc57-155">`InvokeAsync` llama a métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="dbc57-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="dbc57-156">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="dbc57-157">SignalR es asincrónica, así que use `async` y `await` al realizar las llamadas.</span><span class="sxs-lookup"><span data-stu-id="dbc57-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="dbc57-158">El `InvokeAsync` método devuelve un `Task` que se completa cuando finaliza el método de servidor.</span><span class="sxs-lookup"><span data-stu-id="dbc57-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="dbc57-159">El valor devuelto, si existe, se proporciona como el resultado de la `Task`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="dbc57-160">Las excepciones producidas por el método en el servidor generan un error `Task`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="dbc57-161">Use `await` sintaxis que espere a que el método de servidor completar y `try...catch` sintaxis para controlar los errores.</span><span class="sxs-lookup"><span data-stu-id="dbc57-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="dbc57-162">El `SendAsync` método devuelve un `Task` que se completa cuando el mensaje se envió al servidor.</span><span class="sxs-lookup"><span data-stu-id="dbc57-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="dbc57-163">Se proporciona ningún valor devuelto, ya que `Task` no espere hasta que finalice el método de servidor.</span><span class="sxs-lookup"><span data-stu-id="dbc57-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="dbc57-164">Las excepciones producidas en el cliente al enviar el mensaje generan un error `Task`.</span><span class="sxs-lookup"><span data-stu-id="dbc57-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="dbc57-165">Use `await` y `try...catch` sintaxis para controlar errores de envío.</span><span class="sxs-lookup"><span data-stu-id="dbc57-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="dbc57-166">Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="dbc57-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="dbc57-167">Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="dbc57-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="dbc57-168">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="dbc57-168">Call client methods from hub</span></span>

<span data-ttu-id="dbc57-169">Definir métodos que el centro de llamadas utilizando `connection.On` tras su creación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="dbc57-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="dbc57-170">El código anterior en `connection.On` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="dbc57-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="dbc57-171">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="dbc57-171">Error handling and logging</span></span>

<span data-ttu-id="dbc57-172">Controlar los errores con una instrucción try-catch.</span><span class="sxs-lookup"><span data-stu-id="dbc57-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="dbc57-173">Inspeccionar el `Exception` para determinar la acción adecuada que deben realizar después de que se produce un error.</span><span class="sxs-lookup"><span data-stu-id="dbc57-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="dbc57-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dbc57-174">Additional resources</span></span>

* [<span data-ttu-id="dbc57-175">Concentradores</span><span class="sxs-lookup"><span data-stu-id="dbc57-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="dbc57-176">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc57-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="dbc57-177">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="dbc57-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="dbc57-178">Documentación sin servidor de Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="dbc57-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
