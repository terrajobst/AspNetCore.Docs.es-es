---
title: Compatibilidad con WebSockets en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: bba9cf051deaf57efdd82ca2fb1318fce79bd6cc
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223215"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="5bb68-103">Compatibilidad con WebSockets en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5bb68-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="5bb68-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5bb68-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="5bb68-105">En este artículo se ofrece una introducción a WebSockets en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5bb68-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="5bb68-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="5bb68-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="5bb68-107">Se usa en aplicaciones que sacan partido de comunicaciones rápidas y en tiempo real, como las aplicaciones de chat, panel y juegos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="5bb68-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5bb68-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5bb68-109">Para más información, vea la sección [Pasos siguientes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="5bb68-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5bb68-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5bb68-110">Prerequisites</span></span>

* <span data-ttu-id="5bb68-111">ASP.NET Core 1.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="5bb68-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="5bb68-112">Cualquier sistema operativo que admita ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5bb68-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="5bb68-113">Windows 7/Windows Server 2008 o posterior</span><span class="sxs-lookup"><span data-stu-id="5bb68-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="5bb68-114">Linux</span><span class="sxs-lookup"><span data-stu-id="5bb68-114">Linux</span></span>
  * <span data-ttu-id="5bb68-115">macOS</span><span class="sxs-lookup"><span data-stu-id="5bb68-115">macOS</span></span>
  
* <span data-ttu-id="5bb68-116">Si la aplicación se ejecuta en Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="5bb68-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="5bb68-117">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="5bb68-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="5bb68-118">IIS 8/Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="5bb68-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="5bb68-119">WebSockets debe estar habilitado (consulte la sección [Compatibilidad con IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="5bb68-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="5bb68-120">Si la aplicación se ejecuta en [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="5bb68-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="5bb68-121">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="5bb68-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="5bb68-122">Para saber qué exploradores son compatibles, vea https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="5bb68-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="5bb68-123">Cuándo usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="5bb68-123">When to use WebSockets</span></span>

<span data-ttu-id="5bb68-124">Use WebSockets para trabajar directamente con una conexión de socket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="5bb68-125">Por ejemplo, úselo para lograr el mejor rendimiento posible en un juego en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="5bb68-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="5bb68-126">[SignalR de ASP.NET Core](xref:signalr/introduction) es una biblioteca que simplifica la adición de la funcionalidad web en tiempo real a las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="5bb68-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="5bb68-127">Usa WebSockets siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="5bb68-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="5bb68-128">Cómo usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="5bb68-128">How to use WebSockets</span></span>

* <span data-ttu-id="5bb68-129">Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="5bb68-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="5bb68-130">Configure el middleware.</span><span class="sxs-lookup"><span data-stu-id="5bb68-130">Configure the middleware.</span></span>
* <span data-ttu-id="5bb68-131">Acepte las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="5bb68-132">Envíe y reciba mensajes.</span><span class="sxs-lookup"><span data-stu-id="5bb68-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="5bb68-133">Configurar el middleware</span><span class="sxs-lookup"><span data-stu-id="5bb68-133">Configure the middleware</span></span>

<span data-ttu-id="5bb68-134">Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5bb68-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="5bb68-135">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="5bb68-135">The following settings can be configured:</span></span>

* <span data-ttu-id="5bb68-136">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="5bb68-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="5bb68-137">El valor predeterminado es de dos minutos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-137">The default is two minutes.</span></span>
* <span data-ttu-id="5bb68-138">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="5bb68-139">Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="5bb68-140">El valor predeterminado es 4 KB.</span><span class="sxs-lookup"><span data-stu-id="5bb68-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5bb68-141">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="5bb68-141">The following settings can be configured:</span></span>

* <span data-ttu-id="5bb68-142">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="5bb68-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="5bb68-143">El valor predeterminado es de dos minutos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-143">The default is two minutes.</span></span>
* <span data-ttu-id="5bb68-144">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="5bb68-145">Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="5bb68-146">El valor predeterminado es 4 KB.</span><span class="sxs-lookup"><span data-stu-id="5bb68-146">The default is 4 KB.</span></span>
* <span data-ttu-id="5bb68-147">`AllowedOrigins` - Una lista de valores de encabezado de origen permitidos para las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="5bb68-148">De forma predeterminada, se permiten todos los orígenes.</span><span class="sxs-lookup"><span data-stu-id="5bb68-148">By default, all origins are allowed.</span></span> <span data-ttu-id="5bb68-149">Consulte "Restricción de los orígenes de WebSocket" a continuación para obtener información detallada.</span><span class="sxs-lookup"><span data-stu-id="5bb68-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="5bb68-150">Aceptar solicitudes WebSocket</span><span class="sxs-lookup"><span data-stu-id="5bb68-150">Accept WebSocket requests</span></span>

<span data-ttu-id="5bb68-151">En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.</span><span class="sxs-lookup"><span data-stu-id="5bb68-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="5bb68-152">El siguiente ejemplo se corresponde con un momento más adelante en el método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="5bb68-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="5bb68-153">Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="5bb68-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="5bb68-154">Cuando se usa un WebSocket, **debe** mantener la canalización de middleware en ejecución durante la duración de la conexión.</span><span class="sxs-lookup"><span data-stu-id="5bb68-154">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="5bb68-155">Si intenta enviar o recibir un mensaje de WebSocket después de que finalice la canalización de middleware, es posible que obtenga una excepción como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="5bb68-155">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="5bb68-156">Si utiliza un servicio en segundo plano para escribir datos en un WebSocket, asegúrese de mantener en ejecución el canal de middleware.</span><span class="sxs-lookup"><span data-stu-id="5bb68-156">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="5bb68-157">Para ello, utilice un <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="5bb68-157">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="5bb68-158">Pase `TaskCompletionSource` a su servicio de segundo plano y pídale que llame a <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> cuando termine con WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-158">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="5bb68-159">A continuación, `await` la propiedad <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> durante la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5bb68-159">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="5bb68-160">Enviar y recibir mensajes</span><span class="sxs-lookup"><span data-stu-id="5bb68-160">Send and receive messages</span></span>

<span data-ttu-id="5bb68-161">El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y proporciona un objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="5bb68-161">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="5bb68-162">Use el objeto `WebSocket` para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="5bb68-162">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="5bb68-163">El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="5bb68-163">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="5bb68-164">El código recibe un mensaje y devuelve inmediatamente el mismo mensaje.</span><span class="sxs-lookup"><span data-stu-id="5bb68-164">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="5bb68-165">Los mensajes se envían y reciben en un bucle hasta que el cliente cierra la conexión:</span><span class="sxs-lookup"><span data-stu-id="5bb68-165">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="5bb68-166">Cuando la conexión WebSocket se acepta antes de que el bucle comience, la canalización de middleware finaliza.</span><span class="sxs-lookup"><span data-stu-id="5bb68-166">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="5bb68-167">Tras cerrar el socket, se desenreda la canalización.</span><span class="sxs-lookup"><span data-stu-id="5bb68-167">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="5bb68-168">Es decir, la solicitud deja de avanzar en la canalización cuando WebSocket se acepta,</span><span class="sxs-lookup"><span data-stu-id="5bb68-168">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="5bb68-169">pero cuando el bucle termina y el socket se cierra, la solicitud vuelve a recorrer la canalización.</span><span class="sxs-lookup"><span data-stu-id="5bb68-169">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="5bb68-170">Control de las desconexiones del cliente</span><span class="sxs-lookup"><span data-stu-id="5bb68-170">Handle client disconnects</span></span>

<span data-ttu-id="5bb68-171">No se informa automáticamente al servidor cuando el cliente se desconecta debido a la pérdida de conectividad.</span><span class="sxs-lookup"><span data-stu-id="5bb68-171">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="5bb68-172">El servidor recibe un mensaje de desconexión solo si el cliente lo envía, acción que no se puede realizar si se pierde la conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="5bb68-172">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="5bb68-173">Si desea realizar alguna acción cuando eso suceda, establezca un tiempo de expiración después de que no se reciba del cliente dentro de un determinado período.</span><span class="sxs-lookup"><span data-stu-id="5bb68-173">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="5bb68-174">Si el cliente no siempre está enviando mensajes y no quiere que se agote el tiempo de expiración solo porque la conexión está inactiva, haga que el cliente utilice un temporizador para enviar un mensaje de ping cada equis segundos.</span><span class="sxs-lookup"><span data-stu-id="5bb68-174">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="5bb68-175">En el servidor, si aún no ha llegado un mensaje dentro de 2\*X segundos después del anterior, termine la conexión e informe que ha desconectado el cliente.</span><span class="sxs-lookup"><span data-stu-id="5bb68-175">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="5bb68-176">Espere el doble del intervalo de tiempo esperado para dejar tiempo extra para los retrasos de la red que podrían retener el mensaje de ping.</span><span class="sxs-lookup"><span data-stu-id="5bb68-176">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="5bb68-177">Restricción de los orígenes de WebSocket</span><span class="sxs-lookup"><span data-stu-id="5bb68-177">WebSocket origin restriction</span></span>

<span data-ttu-id="5bb68-178">Las protecciones proporcionadas por CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5bb68-178">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="5bb68-179">Los exploradores **no** hacen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5bb68-179">Browsers do **not**:</span></span>

* <span data-ttu-id="5bb68-180">Efectúan solicitudes preparatorias CORS.</span><span class="sxs-lookup"><span data-stu-id="5bb68-180">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="5bb68-181">Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-181">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="5bb68-182">En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5bb68-182">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="5bb68-183">Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.</span><span class="sxs-lookup"><span data-stu-id="5bb68-183">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="5bb68-184">Si hospeda su servidor en "https://server.com" y su cliente en "https://client.com", agregue "https://client.com" a la lista `AllowedOrigins` de WebSockets para efectuar la comprobación.</span><span class="sxs-lookup"><span data-stu-id="5bb68-184">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="5bb68-185">El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar.</span><span class="sxs-lookup"><span data-stu-id="5bb68-185">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="5bb68-186">**No** use estos encabezados como mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="5bb68-186">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="5bb68-187">Compatibilidad con IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="5bb68-187">IIS/IIS Express support</span></span>

<span data-ttu-id="5bb68-188">El protocolo WebSocket se puede usar en Windows Server 2012 o posterior, y en Windows 8 o posterior con IIS o IIS Express 8 o posterior.</span><span class="sxs-lookup"><span data-stu-id="5bb68-188">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="5bb68-189">WebSocket siempre está habilitado cuando se usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="5bb68-189">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="5bb68-190">Habilitación de WebSocket en IIS</span><span class="sxs-lookup"><span data-stu-id="5bb68-190">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="5bb68-191">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 2012 o posterior:</span><span class="sxs-lookup"><span data-stu-id="5bb68-191">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="5bb68-192">Estos pasos no son necesarios cuando se usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="5bb68-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="5bb68-193">Use el asistente **Agregar roles y características** del menú **Administrar** o el vínculo de **Administrador del servidor**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-193">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="5bb68-194">Seleccione **Instalación basada en características o en roles**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-194">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="5bb68-195">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-195">Select **Next**.</span></span>
1. <span data-ttu-id="5bb68-196">Seleccione el servidor que corresponda (el servidor local está seleccionado de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="5bb68-196">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="5bb68-197">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-197">Select **Next**.</span></span>
1. <span data-ttu-id="5bb68-198">Expanda **Servidor web (IIS)** en el árbol **Roles**, expanda **Servidor web** y, por último, expanda **Desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-198">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="5bb68-199">Seleccione **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-199">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="5bb68-200">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-200">Select **Next**.</span></span>
1. <span data-ttu-id="5bb68-201">Si no necesita más características, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-201">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="5bb68-202">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-202">Select **Install**.</span></span>
1. <span data-ttu-id="5bb68-203">Cuando la instalación finalice, haga clic en **Cerrar** para salir del asistente.</span><span class="sxs-lookup"><span data-stu-id="5bb68-203">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="5bb68-204">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 8 o posterior:</span><span class="sxs-lookup"><span data-stu-id="5bb68-204">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="5bb68-205">Estos pasos no son necesarios cuando se usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="5bb68-205">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="5bb68-206">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="5bb68-206">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="5bb68-207">Abra los nodos siguientes: **Internet Information Services** > **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-207">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="5bb68-208">Seleccione la característica **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-208">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="5bb68-209">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5bb68-209">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="5bb68-210">Deshabilitar WebSocket al usar socket.io en Node.js</span><span class="sxs-lookup"><span data-stu-id="5bb68-210">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="5bb68-211">Si usa la compatibilidad de WebSocket en [socket.io](https://socket.io/) en [Node.js](https://nodejs.org/), deshabilite el módulo IIS WebSocket predeterminado, usando para ello el elemento `webSocket` de *web.config* o de *applicationHost.config*. Si este paso no se lleva a cabo, el módulo IIS WebSocket intenta controlar la comunicación de WebSocket en lugar de Node.js y la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5bb68-211">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="5bb68-212">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5bb68-212">Next steps</span></span>

<span data-ttu-id="5bb68-213">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) que acompaña a este artículo es una aplicación de eco.</span><span class="sxs-lookup"><span data-stu-id="5bb68-213">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="5bb68-214">Tiene una página web que realiza las conexiones WebSocket y el servidor reenvía de vuelta al cliente todos los mensajes que reciba.</span><span class="sxs-lookup"><span data-stu-id="5bb68-214">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="5bb68-215">Ejecute la aplicación desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y vaya a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="5bb68-215">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="5bb68-216">En la página web se muestra el estado de conexión en la parte superior izquierda:</span><span class="sxs-lookup"><span data-stu-id="5bb68-216">The web page shows the connection status in the upper left:</span></span>

![Estado inicial de la página web](websockets/_static/start.png)

<span data-ttu-id="5bb68-218">Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="5bb68-218">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="5bb68-219">Escriba un mensaje de prueba y seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="5bb68-219">Enter a test message and select **Send**.</span></span> <span data-ttu-id="5bb68-220">Cuando haya terminado, seleccione **Close Socket** (Cerrar socket).</span><span class="sxs-lookup"><span data-stu-id="5bb68-220">When done, select **Close Socket**.</span></span> <span data-ttu-id="5bb68-221">Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.</span><span class="sxs-lookup"><span data-stu-id="5bb68-221">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial de la página web](websockets/_static/end.png)
