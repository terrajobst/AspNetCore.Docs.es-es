---
title: Cliente ASP.NET Core SignalR JavaScript
author: bradygaster
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: db9a8bbc8f111728f0827e3639e40785149bf79e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899221"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="05061-103">Cliente ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="05061-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="05061-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="05061-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="05061-105">La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="05061-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="05061-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05061-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="05061-107">Instale el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="05061-107">Install the SignalR client package</span></span>

<span data-ttu-id="05061-108">La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete.</span><span class="sxs-lookup"><span data-stu-id="05061-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="05061-109">Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="05061-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="05061-110">Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="05061-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="05061-111">NPM instala el contenido del paquete en el *node_modules\\@aspnet\signalr\dist\browser* carpeta.</span><span class="sxs-lookup"><span data-stu-id="05061-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="05061-112">Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="05061-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="05061-113">Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.</span><span class="sxs-lookup"><span data-stu-id="05061-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="05061-114">Usar al cliente de JavaScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="05061-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="05061-115">Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="05061-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="05061-116">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="05061-116">Connect to a hub</span></span>

<span data-ttu-id="05061-117">El código siguiente se crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="05061-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="05061-118">Nombre del concentrador distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="05061-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="05061-119">Conexiones de origen cruzado</span><span class="sxs-lookup"><span data-stu-id="05061-119">Cross-origin connections</span></span>

<span data-ttu-id="05061-120">Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="05061-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="05061-121">Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="05061-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="05061-122">Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="05061-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="05061-123">Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="05061-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="05061-124">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="05061-124">Call hub methods from client</span></span>

<span data-ttu-id="05061-125">Los clientes JavaScript llamar a métodos públicos en concentradores a través de la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) método de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="05061-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="05061-126">El `invoke` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="05061-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="05061-127">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="05061-127">The name of the hub method.</span></span> <span data-ttu-id="05061-128">En el ejemplo siguiente, que es el nombre del método del concentrador `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="05061-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="05061-129">Los argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="05061-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="05061-130">En el ejemplo siguiente, que es el nombre del argumento `message`.</span><span class="sxs-lookup"><span data-stu-id="05061-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="05061-131">El código de ejemplo usa la sintaxis de la función de flecha que se admiten en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="05061-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="05061-132">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="05061-132">Call client methods from hub</span></span>

<span data-ttu-id="05061-133">Para recibir mensajes desde el centro, defina un método mediante el [en](/javascript/api/%40aspnet/signalr/hubconnection#on) método de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="05061-133">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="05061-134">El nombre del método de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="05061-134">The name of the JavaScript client method.</span></span> <span data-ttu-id="05061-135">En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="05061-135">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="05061-136">Argumentos que del concentrador se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="05061-136">Arguments the hub passes to the method.</span></span> <span data-ttu-id="05061-137">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="05061-137">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="05061-138">El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) método.</span><span class="sxs-lookup"><span data-stu-id="05061-138">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="05061-139">SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="05061-139">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="05061-140">Como práctica recomendada, llame a la [iniciar](/javascript/api/%40aspnet/signalr/hubconnection#start) método en el `HubConnection` después `on`.</span><span class="sxs-lookup"><span data-stu-id="05061-140">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="05061-141">Si lo hace, garantiza que los controladores registrados antes de que se reciben los mensajes.</span><span class="sxs-lookup"><span data-stu-id="05061-141">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="05061-142">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="05061-142">Error handling and logging</span></span>

<span data-ttu-id="05061-143">Cadena de un `catch` método al final de la `start` método para controlar los errores del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="05061-143">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="05061-144">Use `console.error` para errores de salida a la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="05061-144">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="05061-145">Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión.</span><span class="sxs-lookup"><span data-stu-id="05061-145">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="05061-146">Los mensajes se registran con el nivel de registro especificado y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="05061-146">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="05061-147">Los niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="05061-147">Available log levels are as follows:</span></span>

* <span data-ttu-id="05061-148">`signalR.LogLevel.Error` &ndash; Mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="05061-148">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="05061-149">Registros `Error` solo los mensajes.</span><span class="sxs-lookup"><span data-stu-id="05061-149">Logs `Error` messages only.</span></span>
* <span data-ttu-id="05061-150">`signalR.LogLevel.Warning` &ndash; Mensajes de advertencia acerca de posibles errores.</span><span class="sxs-lookup"><span data-stu-id="05061-150">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="05061-151">Los registros de `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="05061-151">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="05061-152">`signalR.LogLevel.Information` &ndash; Mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="05061-152">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="05061-153">Los registros de `Information`, `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="05061-153">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="05061-154">`signalR.LogLevel.Trace` &ndash; Mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="05061-154">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="05061-155">Registra todo, incluidos los datos transportados entre cliente y concentrador.</span><span class="sxs-lookup"><span data-stu-id="05061-155">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="05061-156">Use la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="05061-156">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="05061-157">Los mensajes se registran en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="05061-157">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="05061-158">Volver a conectar los clientes</span><span class="sxs-lookup"><span data-stu-id="05061-158">Reconnect clients</span></span>

<span data-ttu-id="05061-159">El cliente de JavaScript de SignalR no volver a conectar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="05061-159">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="05061-160">Debe escribir código que se volverá a conectar al cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="05061-160">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="05061-161">El código siguiente muestra un enfoque típico de reconexión:</span><span class="sxs-lookup"><span data-stu-id="05061-161">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="05061-162">Una función (en este caso, el `start` función) se crea para iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="05061-162">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="05061-163">Llame a la `start` función en la conexión `onclose` controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="05061-163">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="05061-164">Una implementación real podría usar un retroceso exponencial o vuelva a intentar un número especificado de veces antes de desistir.</span><span class="sxs-lookup"><span data-stu-id="05061-164">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="05061-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="05061-165">Additional resources</span></span>

* [<span data-ttu-id="05061-166">Referencia de API de JavaScript</span><span class="sxs-lookup"><span data-stu-id="05061-166">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="05061-167">Tutorial de JavaScript</span><span class="sxs-lookup"><span data-stu-id="05061-167">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="05061-168">Tutorial de TypeScript y WebPack</span><span class="sxs-lookup"><span data-stu-id="05061-168">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="05061-169">Concentradores</span><span class="sxs-lookup"><span data-stu-id="05061-169">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="05061-170">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="05061-170">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="05061-171">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="05061-171">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="05061-172">Solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="05061-172">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
