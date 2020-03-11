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
ms.openlocfilehash: 3086b4aa532dfe992e19c193ef76f216f7835164
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651539"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="ecfb0-103">ASP.NET Core SignalR cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ecfb0-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="ecfb0-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ecfb0-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="ecfb0-105">La ASP.NET Core SignalR biblioteca de cliente de JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="ecfb0-106">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ecfb0-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="ecfb0-107">Instalar el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="ecfb0-107">Install the SignalR client package</span></span>

<span data-ttu-id="ecfb0-108">La biblioteca de cliente de JavaScript de SignalR se entrega como un paquete [NPM](https://www.npmjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="ecfb0-109">Si usa Visual Studio, ejecute `npm install` desde la consola del **Administrador de paquetes** mientras se encuentra en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="ecfb0-110">Para obtener Visual Studio Code, ejecute el comando desde el **terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="ecfb0-111">NPM instala el contenido del paquete en la carpeta de *@microsoft\signalr\dist\browserde\\de node_modules* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="ecfb0-112">Cree una nueva carpeta denominada *signalr* en la carpeta *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="ecfb0-113">Copie el archivo *signalr. js* en la carpeta *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="ecfb0-114">NPM instala el contenido del paquete en la carpeta de *@aspnet\signalr\dist\browserde\\de node_modules* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="ecfb0-115">Cree una nueva carpeta denominada *signalr* en la carpeta *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="ecfb0-116">Copie el archivo *signalr. js* en la carpeta *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="ecfb0-117">Uso del cliente de SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="ecfb0-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="ecfb0-118">Haga referencia al cliente de SignalR JavaScript en el elemento `<script>`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="ecfb0-119">Conexión a un concentrador</span><span class="sxs-lookup"><span data-stu-id="ecfb0-119">Connect to a hub</span></span>

<span data-ttu-id="ecfb0-120">El código siguiente crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="ecfb0-121">El nombre del centro no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="ecfb0-122">Conexiones entre orígenes</span><span class="sxs-lookup"><span data-stu-id="ecfb0-122">Cross-origin connections</span></span>

<span data-ttu-id="ecfb0-123">Normalmente, los exploradores cargan conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="ecfb0-124">Sin embargo, hay ocasiones en las que se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="ecfb0-125">Para evitar que un sitio malintencionado Lea datos confidenciales de otro sitio, [las conexiones entre orígenes](xref:security/cors) están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="ecfb0-126">Para permitir una solicitud entre orígenes, habilítela en la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ecfb0-127">Llamar a métodos de Hub desde el cliente</span><span class="sxs-lookup"><span data-stu-id="ecfb0-127">Call hub methods from client</span></span>

<span data-ttu-id="ecfb0-128">Los clientes de JavaScript llaman a métodos públicos en los concentradores a través del método [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) de [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="ecfb0-129">El método `invoke` acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="ecfb0-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="ecfb0-130">Nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-130">The name of the hub method.</span></span> <span data-ttu-id="ecfb0-131">En el ejemplo siguiente, el nombre del método en el concentrador es `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="ecfb0-132">Cualquier argumento definido en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="ecfb0-133">En el ejemplo siguiente, el nombre del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="ecfb0-134">El código de ejemplo utiliza la sintaxis de la función de flecha que se admite en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="ecfb0-135">Si usa Azure SignalR Service en modo sin *servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="ecfb0-136">Para obtener más información, consulte la [documentación del servicio deSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="ecfb0-137">El método `invoke` devuelve un [compromiso](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="ecfb0-138">El `Promise` se resuelve con el valor devuelto (si existe) cuando el método en el servidor devuelve.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="ecfb0-139">Si el método en el servidor produce un error, el `Promise` se rechaza con el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="ecfb0-140">Utilice los métodos `then` y `catch` en el `Promise` mismo para controlar estos casos (o la sintaxis `await`).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="ecfb0-141">El método `send` devuelve un `Promise`de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="ecfb0-142">El `Promise` se resuelve cuando el mensaje se envía al servidor.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="ecfb0-143">Si se produce un error al enviar el mensaje, el `Promise` se rechaza con el mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="ecfb0-144">Utilice los métodos `then` y `catch` en el `Promise` mismo para controlar estos casos (o la sintaxis `await`).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="ecfb0-145">El uso de `send` no espera hasta que el servidor haya recibido el mensaje.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="ecfb0-146">Por lo tanto, no es posible devolver datos ni errores del servidor.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ecfb0-147">Llamar a métodos de cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="ecfb0-147">Call client methods from hub</span></span>

<span data-ttu-id="ecfb0-148">Para recibir mensajes desde el concentrador, defina un método mediante el método [on](/javascript/api/%40aspnet/signalr/hubconnection#on) del `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="ecfb0-149">Nombre del método de cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="ecfb0-150">En el ejemplo siguiente, el nombre del método es `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="ecfb0-151">Argumentos que el concentrador pasa al método.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="ecfb0-152">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="ecfb0-153">El código anterior en `connection.on` se ejecuta cuando el código del lado servidor lo llama mediante el método [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="ecfb0-154"> determina el método de cliente al que se debe llamar haciendo coincidir el nombre del método y los argumentos definidos en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="ecfb0-155">Como procedimiento recomendado, llame al método [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) en el `HubConnection` después de `on`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="ecfb0-156">Esto garantiza que los controladores se registren antes de que se reciban los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="ecfb0-157">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="ecfb0-157">Error handling and logging</span></span>

<span data-ttu-id="ecfb0-158">Encadenar un método `catch` al final del método `start` para controlar los errores del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="ecfb0-159">Use `console.error` para generar errores en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="ecfb0-160">Configure el seguimiento del registro del lado cliente pasando un registrador y un tipo de evento al registro cuando se establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="ecfb0-161">Los mensajes se registran con el nivel de registro especificado y superior.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="ecfb0-162">Los niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="ecfb0-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="ecfb0-163">`signalR.LogLevel.Error` &ndash; mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="ecfb0-164">Solo registra mensajes `Error`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="ecfb0-165">`signalR.LogLevel.Warning` &ndash; mensajes de advertencia sobre posibles errores.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="ecfb0-166">Registra `Warning`y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="ecfb0-167">`signalR.LogLevel.Information` &ndash; mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="ecfb0-168">Registra los mensajes de `Information`, `Warning`y `Error`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="ecfb0-169">`signalR.LogLevel.Trace` &ndash; mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="ecfb0-170">Registra todo, incluidos los datos transportados entre el concentrador y el cliente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="ecfb0-171">Use el método [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) en [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="ecfb0-172">Los mensajes se registran en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="ecfb0-173">Volver a conectar clientes</span><span class="sxs-lookup"><span data-stu-id="ecfb0-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="ecfb0-174">Volver a conectar automáticamente</span><span class="sxs-lookup"><span data-stu-id="ecfb0-174">Automatically reconnect</span></span>

<span data-ttu-id="ecfb0-175">El cliente de JavaScript para SignalR se puede configurar para que se vuelva a conectar automáticamente mediante el método de `withAutomaticReconnect` en [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="ecfb0-176">No se volverá a conectar automáticamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="ecfb0-177">Sin ningún parámetro, `withAutomaticReconnect()` configura el cliente para que espere 0, 2, 10 y 30 segundos, respectivamente, antes de intentar cada intento de reconexión, deteniéndose después de cuatro intentos erróneos.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="ecfb0-178">Antes de iniciar cualquier intento de reconexión, la `HubConnection` pasará al estado `HubConnectionState.Reconnecting` y activará sus devoluciones de llamada `onreconnecting` en lugar de pasar al estado `Disconnected` y desencadenar sus `onclose` devoluciones de llamada, como una `HubConnection` sin necesidad de configurar la reconexión automática.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="ecfb0-179">Esto proporciona una oportunidad para advertir a los usuarios de que se ha perdido la conexión y deshabilitar los elementos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="ecfb0-180">Si el cliente se vuelve a conectar correctamente dentro de los cuatro primeros intentos, el `HubConnection` volverá al estado `Connected` y activará sus devoluciones de llamada `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="ecfb0-181">Esto proporciona una oportunidad para informar a los usuarios de que se ha restablecido la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="ecfb0-182">Dado que la conexión es completamente nueva en el servidor, se proporcionará un nuevo `connectionId` a la devolución de llamada `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="ecfb0-183">El parámetro `connectionId` de la devolución de llamada de `onreconnected` será undefined si el `HubConnection` se configuró para [omitir la negociación](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="ecfb0-184">`withAutomaticReconnect()` no configurará el `HubConnection` para reintentar errores de inicio iniciales, por lo que los errores de inicio deben controlarse manualmente:</span><span class="sxs-lookup"><span data-stu-id="ecfb0-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="ecfb0-185">Si el cliente no se vuelve a conectar correctamente en los cuatro primeros intentos, el `HubConnection` pasará al estado `Disconnected` y activará sus devoluciones de llamada [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) .</span><span class="sxs-lookup"><span data-stu-id="ecfb0-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="ecfb0-186">Esto proporciona una oportunidad para informar a los usuarios de que la conexión se ha perdido permanentemente y recomienda actualizar la página:</span><span class="sxs-lookup"><span data-stu-id="ecfb0-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="ecfb0-187">Con el fin de configurar un número personalizado de intentos de reconexión antes de desconectar o cambiar el tiempo de reconexión, `withAutomaticReconnect` acepta una matriz de números que representan el retraso en milisegundos que se debe esperar antes de iniciar cada intento de reconexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="ecfb0-188">En el ejemplo anterior se configura la `HubConnection` para iniciar el intento de reconexión inmediatamente después de la pérdida de la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="ecfb0-189">Esto también se aplica a la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="ecfb0-190">Si se produce un error en el primer intento de reconexión, el segundo intento de reconexión también se iniciará inmediatamente en lugar de esperar 2 segundos como lo haría en la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="ecfb0-191">Si se produce un error en el segundo intento de reconexión, el tercer intento de reconexión se iniciará en 10 segundos, lo que volverá a ser como la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="ecfb0-192">Después, el comportamiento personalizado difiere de nuevo del comportamiento predeterminado al detenerse después de que se produzca un error en el tercer intento de reconexión en lugar de intentar un intento de reconexión más en otros 30 segundos como lo haría en la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="ecfb0-193">Si desea tener un mayor control sobre la temporización y el número de intentos de reconexión automática, `withAutomaticReconnect` acepta un objeto que implementa la interfaz de `IRetryPolicy`, que tiene un único método denominado `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="ecfb0-194">`nextRetryDelayInMilliseconds` toma un solo argumento con el tipo `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="ecfb0-195">El `RetryContext` tiene tres propiedades: `previousRetryCount`, `elapsedMilliseconds` y `retryReason`, que son `number`, `number` y `Error` respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="ecfb0-196">Antes del primer intento de reconexión, tanto `previousRetryCount` como `elapsedMilliseconds` serán cero y el `retryReason` será el error que provocó la pérdida de la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="ecfb0-197">Después de cada intento de reintento erróneo, `previousRetryCount` se incrementará en uno, `elapsedMilliseconds` se actualizará para reflejar la cantidad de tiempo empleado en la reconexión hasta el momento en milisegundos y el `retryReason` será el error que provocó el último intento de reconexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="ecfb0-198">`nextRetryDelayInMilliseconds` debe devolver un número que represente el número de milisegundos de espera antes del siguiente intento de reconexión o `null` si el `HubConnection` debe dejar de volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="ecfb0-199">Como alternativa, puede escribir código que volverá a conectar el cliente manualmente, tal como se muestra en [reconexión manual](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="ecfb0-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="ecfb0-200">Volver a conectar manualmente</span><span class="sxs-lookup"><span data-stu-id="ecfb0-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="ecfb0-201">Antes de 3,0, el cliente de JavaScript para SignalR no se vuelve a conectar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="ecfb0-202">Debe escribir código que volverá a conectar el cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="ecfb0-203">En el código siguiente se muestra un enfoque típico de reconexión manual:</span><span class="sxs-lookup"><span data-stu-id="ecfb0-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="ecfb0-204">Una función (en este caso, la función `start`) se crea para iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="ecfb0-205">Llame a la función `start` en el controlador de eventos `onclose` de la conexión.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="ecfb0-206">Una implementación del mundo real usaría una interrupción exponencial o reintentará un número especificado de veces antes de abandonarlo.</span><span class="sxs-lookup"><span data-stu-id="ecfb0-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecfb0-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ecfb0-207">Additional resources</span></span>

* [<span data-ttu-id="ecfb0-208">Referencia de API de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ecfb0-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="ecfb0-209">Tutorial de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ecfb0-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ecfb0-210">Tutorial de WebPack y TypeScript</span><span class="sxs-lookup"><span data-stu-id="ecfb0-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="ecfb0-211">Concentradores</span><span class="sxs-lookup"><span data-stu-id="ecfb0-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ecfb0-212">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="ecfb0-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="ecfb0-213">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="ecfb0-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="ecfb0-214">Solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="ecfb0-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="ecfb0-215">[Documentación sin servidor del servicio SignalR de Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="ecfb0-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
