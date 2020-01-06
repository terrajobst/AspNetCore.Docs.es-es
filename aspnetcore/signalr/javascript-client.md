---
title: ASP.NET Core SignalR cliente de JavaScript
author: bradygaster
description: Información general de ASP.NET Core SignalR cliente de JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: eaf737642cdbd7ab2b1b5c16538b47a70cddd332
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354699"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET Core SignalR cliente de JavaScript

Por [Rachel Appel](https://twitter.com/rachelappel)

La ASP.NET Core SignalR biblioteca de cliente de JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>Instalar el paquete de cliente de SignalR

La biblioteca de cliente de JavaScript de SignalR se entrega como un paquete [NPM](https://www.npmjs.com/) . Si usa Visual Studio, ejecute `npm install` desde la consola del **Administrador de paquetes** mientras se encuentra en la carpeta raíz. Para obtener Visual Studio Code, ejecute el comando desde el **terminal integrado**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

NPM instala el contenido del paquete en el *node_modules\\@microsoft\signalr\dist\browser* carpeta. Cree una nueva carpeta denominada *signalr* en la carpeta *wwwroot\\lib* . Copie el archivo *signalr. js* en la carpeta *wwwroot\lib\signalr* .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

NPM instala el contenido del paquete en el *node_modules\\@aspnet\signalr\dist\browser* carpeta. Cree una nueva carpeta denominada *signalr* en la carpeta *wwwroot\\lib* . Copie el archivo *signalr. js* en la carpeta *wwwroot\lib\signalr* .

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a>Uso del cliente de SignalR JavaScript

Haga referencia al cliente de SignalR JavaScript en el elemento `<script>`.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conexión a un concentrador

El código siguiente crea e inicia una conexión. El nombre del centro no distingue mayúsculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Conexiones entre orígenes

Normalmente, los exploradores cargan conexiones desde el mismo dominio que la página solicitada. Sin embargo, hay ocasiones en las que se requiere una conexión a otro dominio.

Para evitar que un sitio malintencionado Lea datos confidenciales de otro sitio, [las conexiones entre orígenes](xref:security/cors) están deshabilitadas de forma predeterminada. Para permitir una solicitud entre orígenes, habilítela en la clase `Startup`.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de Hub desde el cliente

Los clientes de JavaScript llaman a métodos públicos en los concentradores a través del método [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). El método `invoke` acepta dos argumentos:

* Nombre del método de concentrador. En el ejemplo siguiente, el nombre del método en el concentrador es `SendMessage`.
* Cualquier argumento definido en el método de concentrador. En el ejemplo siguiente, el nombre del argumento es `message`. El código de ejemplo utiliza la sintaxis de la función de flecha que se admite en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Si usa Azure SignalR Service en modo sin *servidor*, no puede llamar a métodos de concentrador desde un cliente. Para obtener más información, consulte la [documentación del servicio deSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

El método `invoke` devuelve un [compromiso](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)de JavaScript. El `Promise` se resuelve con el valor devuelto (si existe) cuando el método en el servidor devuelve. Si el método en el servidor produce un error, el `Promise` se rechaza con el mensaje de error. Utilice los métodos `then` y `catch` en el `Promise` mismo para controlar estos casos (o la sintaxis `await`).

El método `send` devuelve un `Promise`de JavaScript. El `Promise` se resuelve cuando el mensaje se envía al servidor. Si se produce un error al enviar el mensaje, el `Promise` se rechaza con el mensaje de error. Utilice los métodos `then` y `catch` en el `Promise` mismo para controlar estos casos (o la sintaxis `await`).

> [!NOTE]
> El uso de `send` no espera hasta que el servidor haya recibido el mensaje. Por lo tanto, no es posible devolver datos ni errores del servidor.

## <a name="call-client-methods-from-hub"></a>Llamar a métodos de cliente desde el concentrador

Para recibir mensajes desde el concentrador, defina un método mediante el método [on](/javascript/api/%40aspnet/signalr/hubconnection#on) del `HubConnection`.

* Nombre del método de cliente JavaScript. En el ejemplo siguiente, el nombre del método es `ReceiveMessage`.
* Argumentos que el concentrador pasa al método. En el ejemplo siguiente, el valor del argumento es `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

El código anterior en `connection.on` se ejecuta cuando el código del lado servidor lo llama mediante el método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina el método de cliente al que se debe llamar haciendo coincidir el nombre del método y los argumentos definidos en `SendAsync` y `connection.on`.

> [!NOTE]
> Como procedimiento recomendado, llame al método [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) en el `HubConnection` después de `on`. Esto garantiza que los controladores se registren antes de que se reciban los mensajes.

## <a name="error-handling-and-logging"></a>Registro y control de errores

Encadenar un método `catch` al final del método `start` para controlar los errores del lado cliente. Use `console.error` para generar errores en la consola del explorador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Configure el seguimiento del registro del lado cliente pasando un registrador y un tipo de evento al registro cuando se establece la conexión. Los mensajes se registran con el nivel de registro especificado y superior. Los niveles de registro disponibles son los siguientes:

* `signalR.LogLevel.Error` &ndash; mensajes de error. Solo registra mensajes `Error`.
* `signalR.LogLevel.Warning` &ndash; mensajes de advertencia sobre posibles errores. Registra `Warning`y `Error` mensajes.
* `signalR.LogLevel.Information` &ndash; mensajes de estado sin errores. Registra los mensajes de `Information`, `Warning`y `Error`.
* `signalR.LogLevel.Trace` &ndash; mensajes de seguimiento. Registra todo, incluidos los datos transportados entre el concentrador y el cliente.

Use el método [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) en [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro. Los mensajes se registran en la consola del explorador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Volver a conectar clientes

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Volver a conectar automáticamente

El cliente de JavaScript para SignalR se puede configurar para que se vuelva a conectar automáticamente mediante el método de `withAutomaticReconnect` en [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). No se volverá a conectar automáticamente de forma predeterminada.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Sin ningún parámetro, `withAutomaticReconnect()` configura el cliente para que espere 0, 2, 10 y 30 segundos, respectivamente, antes de intentar cada intento de reconexión, deteniéndose después de cuatro intentos erróneos.

Antes de iniciar cualquier intento de reconexión, la `HubConnection` pasará al estado `HubConnectionState.Reconnecting` y activará sus devoluciones de llamada `onreconnecting` en lugar de pasar al estado `Disconnected` y desencadenar sus `onclose` devoluciones de llamada, como una `HubConnection` sin necesidad de configurar la reconexión automática. Esto proporciona una oportunidad para advertir a los usuarios de que se ha perdido la conexión y deshabilitar los elementos de la interfaz de usuario.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Si el cliente se vuelve a conectar correctamente dentro de los cuatro primeros intentos, el `HubConnection` volverá al estado `Connected` y activará sus devoluciones de llamada `onreconnected`. Esto proporciona una oportunidad para informar a los usuarios de que se ha restablecido la conexión.

Dado que la conexión es completamente nueva en el servidor, se proporcionará un nuevo `connectionId` a la devolución de llamada `onreconnected`.

> [!WARNING]
> El parámetro `connectionId` de la devolución de llamada de `onreconnected` será undefined si el `HubConnection` se configuró para [omitir la negociación](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` no configurará el `HubConnection` para reintentar errores de inicio iniciales, por lo que los errores de inicio deben controlarse manualmente:

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

Si el cliente no se vuelve a conectar correctamente en los cuatro primeros intentos, el `HubConnection` pasará al estado `Disconnected` y activará sus devoluciones de llamada [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Esto proporciona una oportunidad para informar a los usuarios de que la conexión se ha perdido permanentemente y recomienda actualizar la página:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `withAutomaticReconnect` acepta una matriz de números que representan el retraso en milisegundos que se debe esperar antes de iniciar cada intento de reconexión.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

En el ejemplo anterior se configura la `HubConnection` para iniciar el intento de reconexión inmediatamente después de la pérdida de la conexión. Esto también se aplica a la configuración predeterminada.

Si se produce un error en el primer intento de reconexión, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar 2 segundos como lo haría en la configuración predeterminada.

Si se produce un error en el segundo intento de reconexión, el tercer intento de reconexión se iniciará en 10 segundos, lo que volverá a ser como la configuración predeterminada.

Después, el comportamiento personalizado difiere de nuevo del comportamiento predeterminado al detenerse después de que se produzca un error en el tercer intento de reconexión en lugar de intentar un intento de reconexión más en otros 30 segundos como lo haría en la configuración predeterminada.

Si desea tener un mayor control sobre la temporización y el número de intentos de reconexión automática, `withAutomaticReconnect` acepta un objeto que implementa la interfaz de `IRetryPolicy`, que tiene un único método denominado `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` toma un solo argumento con el tipo `RetryContext`. El `RetryContext` tiene tres propiedades: `previousRetryCount`, `elapsedMilliseconds` y `retryReason`, que son `number`, `number` y `Error` respectivamente. Antes del primer intento de reconexión, tanto `previousRetryCount` como `elapsedMilliseconds` serán cero y el `retryReason` será el error que provocó la pérdida de la conexión. Después de cada intento de reintento erróneo, `previousRetryCount` se incrementará en uno, `elapsedMilliseconds` se actualizará para reflejar la cantidad de tiempo empleado en la reconexión hasta el momento en milisegundos y el `retryReason` será el error que provocó el último intento de reconexión.

`nextRetryDelayInMilliseconds` debe devolver un número que represente el número de milisegundos de espera antes del siguiente intento de reconexión o `null` si el `HubConnection` debe dejar de volver a conectarse.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
            if (retryContext.elapsedMilliseconds < 60000) {
                // If we've been reconnecting for less than 60 seconds so far,
                // wait between 0 and 10 seconds before the next reconnect attempt.
                return Math.random() * 10000;
            } else {
                // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
                return null;
            }
        }
    })
    .build();
```

Como alternativa, puede escribir código que volverá a conectar el cliente manualmente, tal como se muestra en [reconexión manual](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Volver a conectar manualmente

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Antes de 3,0, el cliente de JavaScript para SignalR no se vuelve a conectar automáticamente. Debe escribir código que volverá a conectar el cliente manualmente.

::: moniker-end

En el código siguiente se muestra un enfoque típico de reconexión manual:

1. Una función (en este caso, la función `start`) se crea para iniciar la conexión.
1. Llame a la función `start` en el controlador de eventos `onclose` de la conexión.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Una implementación del mundo real usaría una interrupción exponencial o reintentará un número especificado de veces antes de abandonarlo.

## <a name="additional-resources"></a>Recursos adicionales

* [Referencia de API de JavaScript](/javascript/api/?view=signalr-js-latest)
* [Tutorial de JavaScript](xref:tutorials/signalr)
* [Tutorial de WebPack y TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Solicitudes entre orígenes (CORS)](xref:security/cors)
* [Documentación sin servidor del servicio SignalR de Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
