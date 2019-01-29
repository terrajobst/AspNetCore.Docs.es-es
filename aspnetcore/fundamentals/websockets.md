---
title: Compatibilidad con WebSockets en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 76acb9c96ed5e8bbbaf39eeb6cb23307bb44fb8d
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836863"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="d65de-103">Compatibilidad con WebSockets en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d65de-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="d65de-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d65de-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="d65de-105">En este artículo se ofrece una introducción a WebSockets en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d65de-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="d65de-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="d65de-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="d65de-107">Se usa en aplicaciones que sacan partido de comunicaciones rápidas y en tiempo real, como las aplicaciones de chat, panel y juegos.</span><span class="sxs-lookup"><span data-stu-id="d65de-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="d65de-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d65de-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="d65de-109">Para más información, vea la sección [Pasos siguientes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="d65de-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d65de-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d65de-110">Prerequisites</span></span>

* <span data-ttu-id="d65de-111">ASP.NET Core 1.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="d65de-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="d65de-112">Cualquier sistema operativo que admita ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d65de-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="d65de-113">Windows 7/Windows Server 2008 o posterior</span><span class="sxs-lookup"><span data-stu-id="d65de-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="d65de-114">Linux</span><span class="sxs-lookup"><span data-stu-id="d65de-114">Linux</span></span>
  * <span data-ttu-id="d65de-115">macOS</span><span class="sxs-lookup"><span data-stu-id="d65de-115">macOS</span></span>
  
* <span data-ttu-id="d65de-116">Si la aplicación se ejecuta en Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="d65de-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="d65de-117">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d65de-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="d65de-118">IIS 8/Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="d65de-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="d65de-119">WebSockets debe estar habilitado (consulte la sección [Compatibilidad con IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="d65de-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="d65de-120">Si la aplicación se ejecuta en [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="d65de-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="d65de-121">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d65de-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="d65de-122">Para saber qué exploradores son compatibles, vea https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="d65de-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="d65de-123">Cuándo usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="d65de-123">When to use WebSockets</span></span>

<span data-ttu-id="d65de-124">Use WebSockets para trabajar directamente con una conexión de socket.</span><span class="sxs-lookup"><span data-stu-id="d65de-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="d65de-125">Por ejemplo, úselo para lograr el mejor rendimiento posible en un juego en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="d65de-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="d65de-126">[SignalR de ASP.NET Core](xref:signalr/introduction) es una biblioteca que simplifica la adición de la funcionalidad web en tiempo real a las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d65de-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="d65de-127">Usa WebSockets siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="d65de-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="d65de-128">Cómo usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="d65de-128">How to use WebSockets</span></span>

* <span data-ttu-id="d65de-129">Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="d65de-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="d65de-130">Configure el middleware.</span><span class="sxs-lookup"><span data-stu-id="d65de-130">Configure the middleware.</span></span>
* <span data-ttu-id="d65de-131">Acepte las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d65de-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="d65de-132">Envíe y reciba mensajes.</span><span class="sxs-lookup"><span data-stu-id="d65de-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="d65de-133">Configurar el middleware</span><span class="sxs-lookup"><span data-stu-id="d65de-133">Configure the middleware</span></span>

<span data-ttu-id="d65de-134">Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="d65de-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d65de-135">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="d65de-135">The following settings can be configured:</span></span>

* <span data-ttu-id="d65de-136">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="d65de-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="d65de-137">El valor predeterminado es de dos minutos.</span><span class="sxs-lookup"><span data-stu-id="d65de-137">The default is two minutes.</span></span>
* <span data-ttu-id="d65de-138">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="d65de-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="d65de-139">Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="d65de-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="d65de-140">El valor predeterminado es 4 KB.</span><span class="sxs-lookup"><span data-stu-id="d65de-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d65de-141">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="d65de-141">The following settings can be configured:</span></span>

* <span data-ttu-id="d65de-142">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="d65de-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="d65de-143">El valor predeterminado es de dos minutos.</span><span class="sxs-lookup"><span data-stu-id="d65de-143">The default is two minutes.</span></span>
* <span data-ttu-id="d65de-144">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="d65de-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="d65de-145">Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="d65de-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="d65de-146">El valor predeterminado es 4 KB.</span><span class="sxs-lookup"><span data-stu-id="d65de-146">The default is 4 KB.</span></span>
* <span data-ttu-id="d65de-147">`AllowedOrigins` - Una lista de valores de encabezado de origen permitidos para las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d65de-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="d65de-148">De forma predeterminada, se permiten todos los orígenes.</span><span class="sxs-lookup"><span data-stu-id="d65de-148">By default, all origins are allowed.</span></span> <span data-ttu-id="d65de-149">Consulte "Restricción de los orígenes de WebSocket" a continuación para obtener información detallada.</span><span class="sxs-lookup"><span data-stu-id="d65de-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="d65de-150">Aceptar solicitudes WebSocket</span><span class="sxs-lookup"><span data-stu-id="d65de-150">Accept WebSocket requests</span></span>

<span data-ttu-id="d65de-151">En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.</span><span class="sxs-lookup"><span data-stu-id="d65de-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="d65de-152">El siguiente ejemplo se corresponde con un momento más adelante en el método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d65de-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="d65de-153">Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="d65de-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="d65de-154">Enviar y recibir mensajes</span><span class="sxs-lookup"><span data-stu-id="d65de-154">Send and receive messages</span></span>

<span data-ttu-id="d65de-155">El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y proporciona un objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="d65de-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="d65de-156">Use el objeto `WebSocket` para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="d65de-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="d65de-157">El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="d65de-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="d65de-158">El código recibe un mensaje y devuelve inmediatamente el mismo mensaje.</span><span class="sxs-lookup"><span data-stu-id="d65de-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="d65de-159">Los mensajes se envían y reciben en un bucle hasta que el cliente cierra la conexión:</span><span class="sxs-lookup"><span data-stu-id="d65de-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="d65de-160">Cuando la conexión WebSocket se acepta antes de que el bucle comience, la canalización de middleware finaliza.</span><span class="sxs-lookup"><span data-stu-id="d65de-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="d65de-161">Tras cerrar el socket, se desenreda la canalización.</span><span class="sxs-lookup"><span data-stu-id="d65de-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="d65de-162">Es decir, la solicitud deja de avanzar en la canalización cuando WebSocket se acepta,</span><span class="sxs-lookup"><span data-stu-id="d65de-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="d65de-163">pero cuando el bucle termina y el socket se cierra, la solicitud vuelve a recorrer la canalización.</span><span class="sxs-lookup"><span data-stu-id="d65de-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="d65de-164">Control de las desconexiones del cliente</span><span class="sxs-lookup"><span data-stu-id="d65de-164">Handle client disconnects</span></span>

<span data-ttu-id="d65de-165">No se informa automáticamente al servidor cuando el cliente se desconecta debido a la pérdida de conectividad.</span><span class="sxs-lookup"><span data-stu-id="d65de-165">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="d65de-166">El servidor recibe un mensaje de desconexión solo si el cliente lo envía, acción que no se puede realizar si se pierde la conexión a Internet.</span><span class="sxs-lookup"><span data-stu-id="d65de-166">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="d65de-167">Si desea realizar alguna acción cuando eso suceda, establezca un tiempo de expiración después de que no se reciba del cliente dentro de un determinado período.</span><span class="sxs-lookup"><span data-stu-id="d65de-167">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="d65de-168">Si el cliente no siempre está enviando mensajes y no quiere que se agote el tiempo de expiración solo porque la conexión está inactiva, haga que el cliente utilice un temporizador para enviar un mensaje de ping cada equis segundos.</span><span class="sxs-lookup"><span data-stu-id="d65de-168">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="d65de-169">En el servidor, si aún no ha llegado un mensaje dentro de 2\*X segundos después del anterior, termine la conexión e informe que ha desconectado el cliente.</span><span class="sxs-lookup"><span data-stu-id="d65de-169">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="d65de-170">Espere el doble del intervalo de tiempo esperado para dejar tiempo extra para los retrasos de la red que podrían retener el mensaje de ping.</span><span class="sxs-lookup"><span data-stu-id="d65de-170">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="d65de-171">Restricción de los orígenes de WebSocket</span><span class="sxs-lookup"><span data-stu-id="d65de-171">WebSocket origin restriction</span></span>

<span data-ttu-id="d65de-172">Las protecciones proporcionadas por CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d65de-172">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="d65de-173">Los exploradores **no** hacen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d65de-173">Browsers do **not**:</span></span>

* <span data-ttu-id="d65de-174">Efectúan solicitudes preparatorias CORS.</span><span class="sxs-lookup"><span data-stu-id="d65de-174">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="d65de-175">Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d65de-175">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="d65de-176">En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d65de-176">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="d65de-177">Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.</span><span class="sxs-lookup"><span data-stu-id="d65de-177">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="d65de-178">Si hospeda su servidor en "https://server.com" y su cliente en "https://client.com", agregue "https://client.com" a la lista `AllowedOrigins` de WebSockets para efectuar la comprobación.</span><span class="sxs-lookup"><span data-stu-id="d65de-178">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="d65de-179">El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar.</span><span class="sxs-lookup"><span data-stu-id="d65de-179">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="d65de-180">**No** use estos encabezados como mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="d65de-180">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="d65de-181">Compatibilidad con IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="d65de-181">IIS/IIS Express support</span></span>

<span data-ttu-id="d65de-182">El protocolo WebSocket se puede usar en Windows Server 2012 o posterior, y en Windows 8 o posterior con IIS o IIS Express 8 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d65de-182">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="d65de-183">WebSocket siempre está habilitado cuando se usa IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d65de-183">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="d65de-184">Habilitación de WebSocket en IIS</span><span class="sxs-lookup"><span data-stu-id="d65de-184">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="d65de-185">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 2012 o posterior:</span><span class="sxs-lookup"><span data-stu-id="d65de-185">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="d65de-186">Estos pasos no son necesarios cuando se usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="d65de-186">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="d65de-187">Use el asistente **Agregar roles y características** del menú **Administrar** o el vínculo de **Administrador del servidor**.</span><span class="sxs-lookup"><span data-stu-id="d65de-187">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="d65de-188">Seleccione **Instalación basada en características o en roles**.</span><span class="sxs-lookup"><span data-stu-id="d65de-188">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="d65de-189">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d65de-189">Select **Next**.</span></span>
1. <span data-ttu-id="d65de-190">Seleccione el servidor que corresponda (el servidor local está seleccionado de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="d65de-190">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="d65de-191">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d65de-191">Select **Next**.</span></span>
1. <span data-ttu-id="d65de-192">Expanda **Servidor web (IIS)** en el árbol **Roles**, expanda **Servidor web** y, por último, expanda **Desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="d65de-192">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="d65de-193">Seleccione **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="d65de-193">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="d65de-194">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d65de-194">Select **Next**.</span></span>
1. <span data-ttu-id="d65de-195">Si no necesita más características, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d65de-195">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="d65de-196">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="d65de-196">Select **Install**.</span></span>
1. <span data-ttu-id="d65de-197">Cuando la instalación finalice, haga clic en **Cerrar** para salir del asistente.</span><span class="sxs-lookup"><span data-stu-id="d65de-197">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="d65de-198">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 8 o posterior:</span><span class="sxs-lookup"><span data-stu-id="d65de-198">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="d65de-199">Estos pasos no son necesarios cuando se usa IIS Express</span><span class="sxs-lookup"><span data-stu-id="d65de-199">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="d65de-200">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="d65de-200">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="d65de-201">Abra los nodos siguientes: **Internet Information Services** > **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="d65de-201">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="d65de-202">Seleccione la característica **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="d65de-202">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="d65de-203">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d65de-203">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="d65de-204">Deshabilitar WebSocket al usar socket.io en Node.js</span><span class="sxs-lookup"><span data-stu-id="d65de-204">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="d65de-205">Si usa la compatibilidad de WebSocket en [socket.io](https://socket.io/) en [Node.js](https://nodejs.org/), deshabilite el módulo IIS WebSocket predeterminado, usando para ello el elemento `webSocket` de *web.config* o de *applicationHost.config*. Si este paso no se lleva a cabo, el módulo IIS WebSocket intenta controlar la comunicación de WebSocket en lugar de Node.js y la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d65de-205">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="d65de-206">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d65de-206">Next steps</span></span>

<span data-ttu-id="d65de-207">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) que acompaña a este artículo es una aplicación de eco.</span><span class="sxs-lookup"><span data-stu-id="d65de-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="d65de-208">Tiene una página web que realiza las conexiones WebSocket y el servidor reenvía de vuelta al cliente todos los mensajes que reciba.</span><span class="sxs-lookup"><span data-stu-id="d65de-208">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="d65de-209">Ejecute la aplicación desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y vaya a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="d65de-209">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="d65de-210">En la página web se muestra el estado de conexión en la parte superior izquierda:</span><span class="sxs-lookup"><span data-stu-id="d65de-210">The web page shows the connection status in the upper left:</span></span>

![Estado inicial de la página web](websockets/_static/start.png)

<span data-ttu-id="d65de-212">Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="d65de-212">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="d65de-213">Escriba un mensaje de prueba y seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="d65de-213">Enter a test message and select **Send**.</span></span> <span data-ttu-id="d65de-214">Cuando haya terminado, seleccione **Close Socket** (Cerrar socket).</span><span class="sxs-lookup"><span data-stu-id="d65de-214">When done, select **Close Socket**.</span></span> <span data-ttu-id="d65de-215">Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.</span><span class="sxs-lookup"><span data-stu-id="d65de-215">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial de la página web](websockets/_static/end.png)
