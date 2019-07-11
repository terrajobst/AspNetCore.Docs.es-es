---
title: Cliente ASP.NET Core SignalR JavaScript
author: bradygaster
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 8b645304b597db0c37fb9cd8814c493ca1c6ee62
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814962"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="c24e7-103">Cliente ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="c24e7-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="c24e7-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c24e7-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c24e7-105">La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c24e7-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="c24e7-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c24e7-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="c24e7-107">Instale el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="c24e7-107">Install the SignalR client package</span></span>

<span data-ttu-id="c24e7-108">La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete.</span><span class="sxs-lookup"><span data-stu-id="c24e7-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="c24e7-109">Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="c24e7-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="c24e7-110">Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="c24e7-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="c24e7-111">NPM instala el contenido del paquete en el *node_modules\\@aspnet\signalr\dist\browser* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c24e7-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="c24e7-112">Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c24e7-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="c24e7-113">Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c24e7-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="c24e7-114">Usar al cliente de JavaScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="c24e7-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="c24e7-115">Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="c24e7-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c24e7-116">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="c24e7-116">Connect to a hub</span></span>

<span data-ttu-id="c24e7-117">El código siguiente se crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="c24e7-118">Nombre del concentrador distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c24e7-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="c24e7-119">Conexiones de origen cruzado</span><span class="sxs-lookup"><span data-stu-id="c24e7-119">Cross-origin connections</span></span>

<span data-ttu-id="c24e7-120">Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="c24e7-121">Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="c24e7-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="c24e7-122">Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="c24e7-123">Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="c24e7-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c24e7-124">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="c24e7-124">Call hub methods from client</span></span>

<span data-ttu-id="c24e7-125">Los clientes JavaScript llamar a métodos públicos en concentradores a través de la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) método de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="c24e7-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="c24e7-126">El `invoke` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="c24e7-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="c24e7-127">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c24e7-127">The name of the hub method.</span></span> <span data-ttu-id="c24e7-128">En el ejemplo siguiente, que es el nombre del método del concentrador `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="c24e7-129">Los argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c24e7-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="c24e7-130">En el ejemplo siguiente, que es el nombre del argumento `message`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="c24e7-131">El código de ejemplo usa la sintaxis de la función de flecha que se admiten en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c24e7-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="c24e7-132">Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="c24e7-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="c24e7-133">Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="c24e7-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="c24e7-134">El `invoke` método devuelve un JavaScript [promesa](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span><span class="sxs-lookup"><span data-stu-id="c24e7-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="c24e7-135">El `Promise` se resuelve con el valor devuelto (si existe) cuando se devuelve el método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c24e7-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="c24e7-136">Si el método en el servidor produce un error, el `Promise` se rechaza el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="c24e7-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="c24e7-137">Use la `then` y `catch` métodos en el `Promise` para controlar estos casos (o `await` sintaxis).</span><span class="sxs-lookup"><span data-stu-id="c24e7-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="c24e7-138">El `send` método devuelve un JavaScript `Promise`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="c24e7-139">El `Promise` se resuelve cuando se envió el mensaje al servidor.</span><span class="sxs-lookup"><span data-stu-id="c24e7-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="c24e7-140">Si se produce un error al enviar el mensaje, el `Promise` se rechaza el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="c24e7-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="c24e7-141">Use la `then` y `catch` métodos en el `Promise` para controlar estos casos (o `await` sintaxis).</span><span class="sxs-lookup"><span data-stu-id="c24e7-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="c24e7-142">Uso de `send` no espera a que el servidor ha recibido el mensaje.</span><span class="sxs-lookup"><span data-stu-id="c24e7-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="c24e7-143">Por lo tanto, no es posible devolver datos o errores del servidor.</span><span class="sxs-lookup"><span data-stu-id="c24e7-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c24e7-144">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="c24e7-144">Call client methods from hub</span></span>

<span data-ttu-id="c24e7-145">Para recibir mensajes desde el centro, defina un método mediante el [en](/javascript/api/%40aspnet/signalr/hubconnection#on) método de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="c24e7-146">El nombre del método de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c24e7-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="c24e7-147">En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="c24e7-148">Argumentos que del concentrador se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="c24e7-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="c24e7-149">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="c24e7-150">El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) método.</span><span class="sxs-lookup"><span data-stu-id="c24e7-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="c24e7-151">SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="c24e7-152">Como práctica recomendada, llame a la [iniciar](/javascript/api/%40aspnet/signalr/hubconnection#start) método en el `HubConnection` después `on`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="c24e7-153">Si lo hace, garantiza que los controladores registrados antes de que se reciben los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c24e7-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="c24e7-154">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="c24e7-154">Error handling and logging</span></span>

<span data-ttu-id="c24e7-155">Cadena de un `catch` método al final de la `start` método para controlar los errores del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c24e7-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="c24e7-156">Use `console.error` para errores de salida a la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="c24e7-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="c24e7-157">Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="c24e7-158">Los mensajes se registran con el nivel de registro especificado y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="c24e7-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="c24e7-159">Los niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="c24e7-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="c24e7-160">`signalR.LogLevel.Error` &ndash; Mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="c24e7-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="c24e7-161">Registros `Error` solo los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c24e7-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="c24e7-162">`signalR.LogLevel.Warning` &ndash; Mensajes de advertencia acerca de posibles errores.</span><span class="sxs-lookup"><span data-stu-id="c24e7-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="c24e7-163">Los registros de `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="c24e7-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c24e7-164">`signalR.LogLevel.Information` &ndash; Mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="c24e7-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="c24e7-165">Los registros de `Information`, `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="c24e7-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c24e7-166">`signalR.LogLevel.Trace` &ndash; Mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c24e7-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="c24e7-167">Registra todo, incluidos los datos transportados entre cliente y concentrador.</span><span class="sxs-lookup"><span data-stu-id="c24e7-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="c24e7-168">Use la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="c24e7-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="c24e7-169">Los mensajes se registran en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="c24e7-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="c24e7-170">Volver a conectar los clientes</span><span class="sxs-lookup"><span data-stu-id="c24e7-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="c24e7-171">Volver a conectar automáticamente</span><span class="sxs-lookup"><span data-stu-id="c24e7-171">Automatically reconnect</span></span>

<span data-ttu-id="c24e7-172">Se puede configurar el cliente de JavaScript de SignalR para volver a conectar automáticamente con el `withAutomaticReconnect` método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="c24e7-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="c24e7-173">No conectar automáticamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="c24e7-174">Sin parámetros, `withAutomaticReconnect()` configura el cliente que se debe esperar 0, 2, 10 y 30 segundos respectivamente antes de intentar cada intento de volver a conectar, deteniéndose después de cuatro intentos con error.</span><span class="sxs-lookup"><span data-stu-id="c24e7-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="c24e7-175">Antes de iniciar cualquier intento de volver a conectar el `HubConnection` le transición a la `HubConnectionState.Reconnecting` de estado y se activan su `onreconnecting` devoluciones de llamada en lugar de realizar la transición a la `Disconnected` estado y desencadenar su `onclose` devoluciones de llamada como un `HubConnection`sin volver a conectar automático configurado.</span><span class="sxs-lookup"><span data-stu-id="c24e7-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="c24e7-176">Esto proporciona una oportunidad para advertir a los usuarios que se ha perdido la conexión y deshabilitar los elementos de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="c24e7-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c24e7-177">Si el cliente vuelve a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` realizará la transición a la `Connected` de estado y activar su `onreconnected` devoluciones de llamada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="c24e7-178">Esto proporciona una oportunidad para informar a los usuarios que se ha restablecido la conexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="c24e7-179">Puesto que la conexión es completamente nuevo en el servidor, un nuevo `connectionId` se proporcionará el `onreconnected` devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="c24e7-180">El `onreconnected` la devolución de llamada `connectionId` parámetro quedará sin definir si la `HubConnection` se configuró para [omitir negociación](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="c24e7-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c24e7-181">`withAutomaticReconnect()` No configurar el `HubConnection` para volver a intentar errores de inicio inicial, por lo que es necesario controlar manualmente los errores de inicio:</span><span class="sxs-lookup"><span data-stu-id="c24e7-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="c24e7-182">Si el cliente no volver a conectar correctamente dentro de sus primeros cuatro intentos, el `HubConnection` le transición a la `Disconnected` de estado y activar su [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) devoluciones de llamada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="c24e7-183">Esto proporciona una oportunidad para informar a los usuarios la conexión ha perdido permanentemente y recomienda actualizar la página:</span><span class="sxs-lookup"><span data-stu-id="c24e7-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c24e7-184">Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `withAutomaticReconnect` acepta una matriz de números que representa el retraso en milisegundos para esperar antes de iniciar cada intento de volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="c24e7-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="c24e7-185">El ejemplo anterior se configura el `HubConnection` iniciar intentar reconexiones inmediatamente después de que se pierde la conexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="c24e7-186">Esto también es cierto para la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="c24e7-187">Si se produce un error en el primer intento de volver a conectar, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar de 2 segundos como lo haría en la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c24e7-188">Si se produce un error en el segundo intento de volver a conectar, se iniciará al tercer intento de reconexión en 10 segundos, que es nuevo como la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="c24e7-189">El comportamiento personalizado, a continuación, divergirá nuevo del comportamiento predeterminado deteniendo tras la reconexión tercer intento error en lugar de intentar una reconexión más intento en otros 30 segundos como lo haría en la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c24e7-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c24e7-190">Si desea tener un mayor control sobre la planeación y el número de automático volver a conectarse, los intentos de `withAutomaticReconnect` acepta un objeto que implementa el `IReconnectPolicy` interfaz, que tiene un solo método denominado `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="c24e7-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="c24e7-191">`nextRetryDelayInMilliseconds` toma dos argumentos, `previousRetryCount` y `elapsedMilliseconds`, que son ambos números.</span><span class="sxs-lookup"><span data-stu-id="c24e7-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="c24e7-192">Antes del primer intento de reconexión ambos `previousRetryCount` y `elapsedMilliseconds` será igual cero.</span><span class="sxs-lookup"><span data-stu-id="c24e7-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="c24e7-193">Después de cada intento de reintento con errores, `previousRetryCount` aumentará en uno y `elapsedMilliseconds` se actualizará para reflejar la cantidad de tiempo empleado en volver a conectarse hasta ahora en milisegundos.</span><span class="sxs-lookup"><span data-stu-id="c24e7-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="c24e7-194">`nextRetryDelayInMilliseconds` debe devolver un número que representa el número de milisegundos para esperar antes del siguiente intento de volver a conectar o `null` si el `HubConnection` debe detenerse la reconexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="c24e7-195">Como alternativa, puede escribir código que se volverá a conectar el cliente manualmente como se muestra en [conectar manualmente](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="c24e7-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="c24e7-196">Volver a conectar manualmente</span><span class="sxs-lookup"><span data-stu-id="c24e7-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="c24e7-197">Antes de 3.0, el cliente de JavaScript de SignalR no volver a conectar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c24e7-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="c24e7-198">Debe escribir código que se volverá a conectar al cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="c24e7-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="c24e7-199">El código siguiente muestra un enfoque típico reconexión manual:</span><span class="sxs-lookup"><span data-stu-id="c24e7-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="c24e7-200">Una función (en este caso, el `start` función) se crea para iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="c24e7-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="c24e7-201">Llame a la `start` función en la conexión `onclose` controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="c24e7-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="c24e7-202">Una implementación real podría usar un retroceso exponencial o vuelva a intentar un número especificado de veces antes de desistir.</span><span class="sxs-lookup"><span data-stu-id="c24e7-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c24e7-203">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c24e7-203">Additional resources</span></span>

* [<span data-ttu-id="c24e7-204">Referencia de API de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c24e7-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="c24e7-205">Tutorial de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c24e7-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="c24e7-206">Tutorial de TypeScript y WebPack</span><span class="sxs-lookup"><span data-stu-id="c24e7-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="c24e7-207">Concentradores</span><span class="sxs-lookup"><span data-stu-id="c24e7-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c24e7-208">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="c24e7-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c24e7-209">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="c24e7-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c24e7-210">Solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="c24e7-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="c24e7-211">Documentación sin servidor de Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="c24e7-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
