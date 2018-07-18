---
title: Cliente ASP.NET Core SignalR JavaScript
author: tdykstra
description: Información general de cliente de JavaScript de SignalR de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095429"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="c1d6c-103">Cliente ASP.NET Core SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="c1d6c-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="c1d6c-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c1d6c-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c1d6c-105">La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="c1d6c-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1d6c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="c1d6c-107">Instale el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="c1d6c-107">Install the SignalR client package</span></span>

<span data-ttu-id="c1d6c-108">La biblioteca de cliente de JavaScript de SignalR se entrega como un [npm](https://www.npmjs.com/) paquete.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="c1d6c-109">Si usa Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="c1d6c-110">Para Visual Studio Code, ejecute el comando desde el **Terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="c1d6c-111">NPM instala el contenido del paquete en el *node_modules\\ @aspnet\signalr\dist\browser*  carpeta.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="c1d6c-112">Cree una carpeta nueva denominada *signalr* bajo el *wwwroot\\lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="c1d6c-113">Copia el *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="c1d6c-114">Usar al cliente de JavaScript de SignalR</span><span class="sxs-lookup"><span data-stu-id="c1d6c-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="c1d6c-115">Referencia del cliente de JavaScript de SignalR en el `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c1d6c-116">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="c1d6c-116">Connect to a hub</span></span>

<span data-ttu-id="c1d6c-117">El código siguiente se crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="c1d6c-118">Nombre del concentrador distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="c1d6c-119">Conexiones de origen cruzado</span><span class="sxs-lookup"><span data-stu-id="c1d6c-119">Cross-origin connections</span></span>

<span data-ttu-id="c1d6c-120">Normalmente, exploradores de carga de las conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="c1d6c-121">Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="c1d6c-122">Para evitar que un sitio malintencionado lea datos confidenciales de otro sitio, [las conexiones de origen cruzado](xref:security/cors) están deshabilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="c1d6c-123">Para permitir que una solicitud de origen cruzado, habilitarlo en el `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c1d6c-124">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="c1d6c-124">Call hub methods from client</span></span>

<span data-ttu-id="c1d6c-125">Los clientes JavaScript llamar a métodos públicos en concentradores por mediante `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="c1d6c-126">El `invoke` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="c1d6c-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="c1d6c-127">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-127">The name of the hub method.</span></span> <span data-ttu-id="c1d6c-128">En el ejemplo siguiente, que es el nombre del centro `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="c1d6c-129">Los argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="c1d6c-130">En el ejemplo siguiente, que es el nombre del argumento `message`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c1d6c-131">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="c1d6c-131">Call client methods from hub</span></span>

<span data-ttu-id="c1d6c-132">Para recibir mensajes desde el centro, defina un método mediante el `connection.on` método.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="c1d6c-133">El nombre del método de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="c1d6c-134">En el ejemplo siguiente, que es el nombre del método `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="c1d6c-135">Argumentos que del concentrador se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="c1d6c-136">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="c1d6c-137">El código anterior en `connection.on` se ejecuta cuando se llama al código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="c1d6c-138">SignalR determina qué método de cliente para llamar a haciendo coincidir el nombre del método y los argumentos definan en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="c1d6c-139">Como práctica recomendada, llame a `connection.start` después `connection.on` por lo que se registran los controladores antes de que se reciben los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="c1d6c-140">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="c1d6c-140">Error handling and logging</span></span>

<span data-ttu-id="c1d6c-141">Cadena de un `catch` método al final de la `connection.start` método para controlar los errores del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="c1d6c-142">Use `console.error` para errores de salida a la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="c1d6c-143">Seguimiento de registro del lado cliente instalación pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="c1d6c-144">Los mensajes se registran con el nivel de registro especificado y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="c1d6c-145">Los niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="c1d6c-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="c1d6c-146">`signalR.LogLevel.Error` : Mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="c1d6c-147">Registros `Error` solo los mensajes.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="c1d6c-148">`signalR.LogLevel.Warning` : Mensajes de advertencia acerca de posibles errores.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="c1d6c-149">Los registros de `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c1d6c-150">`signalR.LogLevel.Information` : Mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="c1d6c-151">Los registros de `Information`, `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c1d6c-152">`signalR.LogLevel.Trace` : Los mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="c1d6c-153">Registra todo, incluidos los datos transportados entre cliente y concentrador.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="c1d6c-154">Use la `configureLogging` método `HubConnectionBuilder` para configurar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="c1d6c-155">Los mensajes se registran en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="c1d6c-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="c1d6c-156">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="c1d6c-156">Related resources</span></span>

* [<span data-ttu-id="c1d6c-157">Concentradores</span><span class="sxs-lookup"><span data-stu-id="c1d6c-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c1d6c-158">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="c1d6c-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c1d6c-159">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="c1d6c-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c1d6c-160">Habilitar solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1d6c-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
