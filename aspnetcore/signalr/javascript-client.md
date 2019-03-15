---
title: Cliente ASP.NET Core SignalR JavaScript
author: bradygaster
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/javascript-client
ms.openlocfilehash: a0980dca2eb8d483a9d9f1c5667fb74ee06364f0
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978347"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="90ad5-103">Cliente ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="90ad5-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="90ad5-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="90ad5-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="90ad5-105">La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="90ad5-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="90ad5-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="90ad5-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="90ad5-107">Instale el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="90ad5-107">Install the SignalR client package</span></span>

<span data-ttu-id="90ad5-108">La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete.</span><span class="sxs-lookup"><span data-stu-id="90ad5-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="90ad5-109">Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="90ad5-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="90ad5-110">Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="90ad5-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="90ad5-111">NPM instala el contenido del paquete en el *node_modules\\@aspnet\signalr\dist\browser* carpeta.</span><span class="sxs-lookup"><span data-stu-id="90ad5-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="90ad5-112">Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="90ad5-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="90ad5-113">Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.</span><span class="sxs-lookup"><span data-stu-id="90ad5-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="90ad5-114">Usar al cliente de JavaScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="90ad5-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="90ad5-115">Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="90ad5-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="90ad5-116">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="90ad5-116">Connect to a hub</span></span>

<span data-ttu-id="90ad5-117">El código siguiente se crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="90ad5-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="90ad5-118">Nombre del concentrador distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="90ad5-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="90ad5-119">Conexiones de origen cruzado</span><span class="sxs-lookup"><span data-stu-id="90ad5-119">Cross-origin connections</span></span>

<span data-ttu-id="90ad5-120">Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="90ad5-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="90ad5-121">Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="90ad5-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="90ad5-122">Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="90ad5-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="90ad5-123">Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="90ad5-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="90ad5-124">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="90ad5-124">Call hub methods from client</span></span>

<span data-ttu-id="90ad5-125">Los clientes JavaScript llamar a métodos públicos en concentradores a través de la [invocar](/javascript/api/%40aspnet/signalr/hubconnection#invoke) método de la [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="90ad5-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="90ad5-126">El `invoke` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="90ad5-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="90ad5-127">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="90ad5-127">The name of the hub method.</span></span> <span data-ttu-id="90ad5-128">En el ejemplo siguiente, que es el nombre del método del concentrador `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="90ad5-129">Los argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="90ad5-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="90ad5-130">En el ejemplo siguiente, que es el nombre del argumento `message`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="90ad5-131">El código de ejemplo usa la sintaxis de la función de flecha que se admiten en las versiones actuales de todos los exploradores principales, excepto Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="90ad5-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="90ad5-132">Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="90ad5-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="90ad5-133">Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="90ad5-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="90ad5-134">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="90ad5-134">Call client methods from hub</span></span>

<span data-ttu-id="90ad5-135">Para recibir mensajes desde el centro, defina un método mediante el [en](/javascript/api/%40aspnet/signalr/hubconnection#on) método de la `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-135">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="90ad5-136">El nombre del método de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="90ad5-136">The name of the JavaScript client method.</span></span> <span data-ttu-id="90ad5-137">En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-137">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="90ad5-138">Argumentos que del concentrador se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="90ad5-138">Arguments the hub passes to the method.</span></span> <span data-ttu-id="90ad5-139">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-139">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="90ad5-140">El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) método.</span><span class="sxs-lookup"><span data-stu-id="90ad5-140">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="90ad5-141">SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-141">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="90ad5-142">Como práctica recomendada, llame a la [iniciar](/javascript/api/%40aspnet/signalr/hubconnection#start) método en el `HubConnection` después `on`.</span><span class="sxs-lookup"><span data-stu-id="90ad5-142">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="90ad5-143">Si lo hace, garantiza que los controladores registrados antes de que se reciben los mensajes.</span><span class="sxs-lookup"><span data-stu-id="90ad5-143">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="90ad5-144">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="90ad5-144">Error handling and logging</span></span>

<span data-ttu-id="90ad5-145">Cadena de un `catch` método al final de la `start` método para controlar los errores del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="90ad5-145">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="90ad5-146">Use `console.error` para errores de salida a la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="90ad5-146">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="90ad5-147">Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión.</span><span class="sxs-lookup"><span data-stu-id="90ad5-147">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="90ad5-148">Los mensajes se registran con el nivel de registro especificado y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="90ad5-148">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="90ad5-149">Los niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="90ad5-149">Available log levels are as follows:</span></span>

* <span data-ttu-id="90ad5-150">`signalR.LogLevel.Error` &ndash; Mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="90ad5-150">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="90ad5-151">Registros `Error` solo los mensajes.</span><span class="sxs-lookup"><span data-stu-id="90ad5-151">Logs `Error` messages only.</span></span>
* <span data-ttu-id="90ad5-152">`signalR.LogLevel.Warning` &ndash; Mensajes de advertencia acerca de posibles errores.</span><span class="sxs-lookup"><span data-stu-id="90ad5-152">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="90ad5-153">Los registros de `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="90ad5-153">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="90ad5-154">`signalR.LogLevel.Information` &ndash; Mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="90ad5-154">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="90ad5-155">Los registros de `Information`, `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="90ad5-155">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="90ad5-156">`signalR.LogLevel.Trace` &ndash; Mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="90ad5-156">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="90ad5-157">Registra todo, incluidos los datos transportados entre cliente y concentrador.</span><span class="sxs-lookup"><span data-stu-id="90ad5-157">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="90ad5-158">Use la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) método [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) para configurar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="90ad5-158">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="90ad5-159">Los mensajes se registran en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="90ad5-159">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="90ad5-160">Volver a conectar los clientes</span><span class="sxs-lookup"><span data-stu-id="90ad5-160">Reconnect clients</span></span>

<span data-ttu-id="90ad5-161">El cliente de JavaScript de SignalR no volver a conectar automáticamente.</span><span class="sxs-lookup"><span data-stu-id="90ad5-161">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="90ad5-162">Debe escribir código que se volverá a conectar al cliente manualmente.</span><span class="sxs-lookup"><span data-stu-id="90ad5-162">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="90ad5-163">El código siguiente muestra un enfoque típico de reconexión:</span><span class="sxs-lookup"><span data-stu-id="90ad5-163">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="90ad5-164">Una función (en este caso, el `start` función) se crea para iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="90ad5-164">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="90ad5-165">Llame a la `start` función en la conexión `onclose` controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="90ad5-165">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="90ad5-166">Una implementación real podría usar un retroceso exponencial o vuelva a intentar un número especificado de veces antes de desistir.</span><span class="sxs-lookup"><span data-stu-id="90ad5-166">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="90ad5-167">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="90ad5-167">Additional resources</span></span>

* [<span data-ttu-id="90ad5-168">Referencia de API de JavaScript</span><span class="sxs-lookup"><span data-stu-id="90ad5-168">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="90ad5-169">Tutorial de JavaScript</span><span class="sxs-lookup"><span data-stu-id="90ad5-169">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="90ad5-170">Tutorial de TypeScript y WebPack</span><span class="sxs-lookup"><span data-stu-id="90ad5-170">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="90ad5-171">Concentradores</span><span class="sxs-lookup"><span data-stu-id="90ad5-171">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="90ad5-172">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="90ad5-172">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="90ad5-173">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="90ad5-173">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="90ad5-174">Solicitudes entre orígenes (CORS)</span><span class="sxs-lookup"><span data-stu-id="90ad5-174">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="90ad5-175">Documentación sin servidor de Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="90ad5-175">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
