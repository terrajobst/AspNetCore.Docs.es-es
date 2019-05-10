---
title: Cliente ASP.NET Core SignalR JavaScript
author: bradygaster
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 8ac6528b203831f426feabf4cdd3e0ed293b75c5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896532"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Cliente ASP.NET Core SignalR JavaScript

Por [Rachel Appel](http://twitter.com/rachelappel)

La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Instale el paquete de cliente de SignalR

La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete. Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz. Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

NPM instala el contenido del paquete en el *node_modules\\@aspnet\signalr\dist\browser* carpeta. Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta. Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.

## <a name="use-the-signalr-javascript-client"></a>Usar al cliente de JavaScript de SignalR

Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

El código siguiente se crea e inicia una conexión. Nombre del concentrador distingue mayúsculas de minúsculas.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Conexiones de origen cruzado

Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada. Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.

Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada. Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

Los clientes JavaScript llamar a métodos públicos en concentradores a través de la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) método de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). El `invoke` método acepta dos argumentos:

* El nombre del método de concentrador. En el ejemplo siguiente, que es el nombre del método del concentrador `SendMessage`.
* Los argumentos definidos en el método de concentrador. En el ejemplo siguiente, que es el nombre del argumento `message`. El código de ejemplo usa la sintaxis de la función de flecha que se admiten en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente. Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

El `invoke` método devuelve un JavaScript [promesa](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). El `Promise` se resuelve con el valor devuelto (si existe) cuando se devuelve el método en el servidor. Si el método en el servidor produce un error, el `Promise` se rechaza el mensaje de error. Use la `then` y `catch` métodos en el `Promise` para controlar estos casos (o `await` sintaxis).

El `send` método devuelve un JavaScript `Promise`. El `Promise` se resuelve cuando se envió el mensaje al servidor. Si se produce un error al enviar el mensaje, el `Promise` se rechaza el mensaje de error. Use la `then` y `catch` métodos en el `Promise` para controlar estos casos (o `await` sintaxis).

> [!NOTE]
> Uso de `send` no espera a que el servidor ha recibido el mensaje. Por lo tanto, no es posible devolver datos o errores del servidor.

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Para recibir mensajes desde el centro, defina un método mediante el [en](/javascript/api/%40aspnet/signalr/hubconnection#on) método de la `HubConnection`.

* El nombre del método de cliente de JavaScript. En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.
* Argumentos que del concentrador se pasa al método. En el ejemplo siguiente, el valor del argumento es `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) método.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.

> [!NOTE]
> Como práctica recomendada, llame a la [iniciar](/javascript/api/%40aspnet/signalr/hubconnection#start) método en el `HubConnection` después `on`. Si lo hace, garantiza que los controladores registrados antes de que se reciben los mensajes.

## <a name="error-handling-and-logging"></a>Registro y control de errores

Cadena de un `catch` método al final de la `start` método para controlar los errores del lado cliente. Use `console.error` para errores de salida a la consola del explorador.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión. Los mensajes se registran con el nivel de registro especificado y versiones posteriores. Los niveles de registro disponibles son los siguientes:

* `signalR.LogLevel.Error` &ndash; Mensajes de error. Registros `Error` solo los mensajes.
* `signalR.LogLevel.Warning` &ndash; Mensajes de advertencia acerca de posibles errores. Los registros de `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Information` &ndash; Mensajes de estado sin errores. Los registros de `Information`, `Warning`, y `Error` mensajes.
* `signalR.LogLevel.Trace` &ndash; Mensajes de seguimiento. Registra todo, incluidos los datos transportados entre cliente y concentrador.

Use la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro. Los mensajes se registran en la consola del explorador.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Volver a conectar los clientes

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Volver a conectar automáticamente

Se puede configurar el cliente de JavaScript de SignalR para volver a conectar automáticamente con el `withAutomaticReconnect` método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). No conectar automáticamente de forma predeterminada.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Sin parámetros, `withAutomaticReconnect()` configura el cliente que se debe esperar 0, 2, 10 y 30 segundos respectivamente antes de intentar cada intento de volver a conectar, deteniéndose después de cuatro intentos con error.

Antes de iniciar cualquier intento de volver a conectar el `HubConnection` le transición a la `HubConnectionState.Reconnecting` de estado y se activan su `onreconnecting` devoluciones de llamada en lugar de realizar la transición a la `Disconnected` estado y desencadenar su `onclose` devoluciones de llamada como un `HubConnection`sin volver a conectar automático configurado. Esto proporciona una oportunidad para advertir a los usuarios que se ha perdido la conexión y deshabilitar los elementos de interfaz de usuario.

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

Si el cliente vuelve a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` realizará la transición a la `Connected` de estado y activar su `onreconnected` devoluciones de llamada. Esto proporciona una oportunidad para informar a los usuarios que se ha restablecido la conexión.

Puesto que la conexión es completamente nuevo en el servidor, un nuevo `connectionId` se proporcionará el `onreconnected` devolución de llamada.

> [!WARNING]
> El `onreconnected` la devolución de llamada `connectionId` parámetro quedará sin definir si la `HubConnection` se configuró para [omitir negociación](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` No configurar el `HubConnection` para volver a intentar errores de inicio inicial, por lo que es necesario controlar manualmente los errores de inicio:

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

Si el cliente no volver a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` le transición a la `Disconnected` de estado y activar su [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) devoluciones de llamada. Esto proporciona una oportunidad para informar a los usuarios la conexión ha perdido permanentemente y recomienda actualizar la página:

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `withAutomaticReconnect` acepta una matriz de números que representa el retraso en milisegundos para esperar antes de iniciar cada intento de volver a conectar.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

El ejemplo anterior se configura el `HubConnection` iniciar intentar reconexiones inmediatamente después de que se pierde la conexión. Esto también es cierto para la configuración predeterminada.

Si se produce un error en el primer intento de volver a conectar, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar de 2 segundos como lo haría en la configuración predeterminada.

Si se produce un error en el segundo intento de volver a conectar, se iniciará al tercer intento de reconexión en 10 segundos, que es nuevo como la configuración predeterminada.

El comportamiento personalizado, a continuación, divergirá nuevo del comportamiento predeterminado deteniendo tras la reconexión tercer intento error en lugar de intentar una reconexión más intento en otros 30 segundos como lo haría en la configuración predeterminada.

Si desea tener un mayor control sobre la planeación y el número de automático volver a conectarse, los intentos de `withAutomaticReconnect` acepta un objeto que implementa el `IReconnectPolicy` interfaz, que tiene un solo método denominado `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` toma dos argumentos, `previousRetryCount` y `elapsedMilliseconds`, que son ambos números. Antes del primer intento de reconexión ambos `previousRetryCount` y `elapsedMilliseconds` será igual cero. Después de cada intento de reintento con errores, `previousRetryCount` aumentará en uno y `elapsedMilliseconds` se actualizará para reflejar la cantidad de tiempo empleado en volver a conectarse hasta ahora en milisegundos.

`nextRetryDelayInMilliseconds` debe devolver un número que representa el número de milisegundos para esperar antes del siguiente intento de volver a conectar o `null` si el `HubConnection` debe detenerse la reconexión.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

Como alternativa, puede escribir código que se volverá a conectar el cliente manualmente como se muestra en [conectar manualmente](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Volver a conectar manualmente

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Antes de 3.0, el cliente de JavaScript de SignalR no volver a conectar automáticamente. Debe escribir código que se volverá a conectar al cliente manualmente.

::: moniker-end

El código siguiente muestra un enfoque típico reconexión manual:

1. Una función (en este caso, el `start` función) se crea para iniciar la conexión.
1. Llame a la `start` función en la conexión `onclose` controlador de eventos.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Una implementación real podría usar un retroceso exponencial o vuelva a intentar un número especificado de veces antes de desistir. 

## <a name="additional-resources"></a>Recursos adicionales

* [Referencia de API de JavaScript](/javascript/api/?view=signalr-js-latest)
* [Tutorial de JavaScript](xref:tutorials/signalr)
* [Tutorial de TypeScript y WebPack](xref:tutorials/signalr-typescript-webpack)
* [Concentradores](xref:signalr/hubs)
* [Cliente .NET](xref:signalr/dotnet-client)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Solicitudes entre orígenes (CORS)](xref:security/cors)
* [Documentación sin servidor de Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
