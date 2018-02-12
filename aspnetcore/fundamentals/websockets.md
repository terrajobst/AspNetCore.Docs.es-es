---
title: Compatibilidad con WebSockets en ASP.NET Core
author: tdykstra
description: "Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="39d62-103">Introducción a WebSockets en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39d62-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="39d62-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="39d62-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="39d62-105">En este artículo se ofrece una introducción a WebSockets en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39d62-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="39d62-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="39d62-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="39d62-107">Se usa para aplicaciones como: chat, tableros de cotizaciones, juegos y cualquier otra aplicación donde se necesiten funciones en tiempo real en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="39d62-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="39d62-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="39d62-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="39d62-109">Para más información, vea la sección [Pasos siguientes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="39d62-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="39d62-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="39d62-110">Prerequisites</span></span>

* <span data-ttu-id="39d62-111">ASP.NET Core 1.1 (no se ejecuta en 1.0)</span><span class="sxs-lookup"><span data-stu-id="39d62-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="39d62-112">Cualquier sistema operativo en el que se pueda ejecutar ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="39d62-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="39d62-113">Windows 7/Windows Server 2008 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="39d62-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="39d62-114">Linux</span><span class="sxs-lookup"><span data-stu-id="39d62-114">Linux</span></span>
  * <span data-ttu-id="39d62-115">macOS</span><span class="sxs-lookup"><span data-stu-id="39d62-115">macOS</span></span>

* <span data-ttu-id="39d62-116">**Excepción**: si la aplicación se ejecuta en Windows con IIS o con WebListener, debe usar:</span><span class="sxs-lookup"><span data-stu-id="39d62-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="39d62-117">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="39d62-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="39d62-118">IIS 8/Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="39d62-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="39d62-119">WebSocket debe estar habilitado en IIS</span><span class="sxs-lookup"><span data-stu-id="39d62-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="39d62-120">Para conocer los exploradores admitidos, vea http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="39d62-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="39d62-121">Cuándo usarlo</span><span class="sxs-lookup"><span data-stu-id="39d62-121">When to use it</span></span>

<span data-ttu-id="39d62-122">Use WebSockets cuando necesite trabajar directamente con una conexión de socket.</span><span class="sxs-lookup"><span data-stu-id="39d62-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="39d62-123">Por ejemplo, suponga que necesita lograr el mejor rendimiento posible para un juego en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="39d62-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="39d62-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) proporciona un modelo de aplicación más completo para funciones en tiempo real, pero solo se ejecuta en ASP.NET, no en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39d62-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="39d62-125">Una versión Core de SignalR está en desarrollo; para seguir su progreso, vea el [repositorio de GitHub para SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="39d62-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="39d62-126">Si no quiere esperar al lanzamiento de SignalR Core, puede usar WebSockets directamente.</span><span class="sxs-lookup"><span data-stu-id="39d62-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="39d62-127">Pero es posible que tenga que desarrollar características que podría obtener en SignalR, como:</span><span class="sxs-lookup"><span data-stu-id="39d62-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="39d62-128">Compatibilidad con una gama más amplia de versiones del explorador, ya que usa la reserva automática para los métodos de transporte alternativos.</span><span class="sxs-lookup"><span data-stu-id="39d62-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="39d62-129">Reconexión automática cuando se produce un fallo de conexión.</span><span class="sxs-lookup"><span data-stu-id="39d62-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="39d62-130">Compatibilidad con clientes que llaman a métodos en el servidor o viceversa.</span><span class="sxs-lookup"><span data-stu-id="39d62-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="39d62-131">Compatibilidad con el escalado a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="39d62-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="39d62-132">Cómo se usa</span><span class="sxs-lookup"><span data-stu-id="39d62-132">How to use it</span></span>

* <span data-ttu-id="39d62-133">Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="39d62-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="39d62-134">Configure el middleware.</span><span class="sxs-lookup"><span data-stu-id="39d62-134">Configure the middleware.</span></span>
* <span data-ttu-id="39d62-135">Acepte las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="39d62-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="39d62-136">Envíe y reciba mensajes.</span><span class="sxs-lookup"><span data-stu-id="39d62-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="39d62-137">Configurar el middleware</span><span class="sxs-lookup"><span data-stu-id="39d62-137">Configure the middleware</span></span>

<span data-ttu-id="39d62-138">Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="39d62-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="39d62-139">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="39d62-139">The following settings can be configured:</span></span>

* <span data-ttu-id="39d62-140">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="39d62-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="39d62-141">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="39d62-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="39d62-142">Solo los usuarios avanzados tendrán que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="39d62-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="39d62-143">Aceptar solicitudes WebSocket</span><span class="sxs-lookup"><span data-stu-id="39d62-143">Accept WebSocket requests</span></span>

<span data-ttu-id="39d62-144">En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.</span><span class="sxs-lookup"><span data-stu-id="39d62-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="39d62-145">Este ejemplo se muestra más adelante en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="39d62-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="39d62-146">Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="39d62-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="39d62-147">Enviar y recibir mensajes</span><span class="sxs-lookup"><span data-stu-id="39d62-147">Send and receive messages</span></span>

<span data-ttu-id="39d62-148">El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y ofrece un objeto [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="39d62-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="39d62-149">Use el objeto WebSocket para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="39d62-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="39d62-150">El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`; aquí se trata del método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="39d62-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="39d62-151">El código recibe un mensaje y devuelve inmediatamente el mismo mensaje.</span><span class="sxs-lookup"><span data-stu-id="39d62-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="39d62-152">Se mantiene en un bucle realizando estas acciones hasta que el cliente cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="39d62-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="39d62-153">Cuando se acepta el WebSocket antes de empezar este bucle, se finaliza la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="39d62-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="39d62-154">Tras cerrar el socket, se desenreda la canalización.</span><span class="sxs-lookup"><span data-stu-id="39d62-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="39d62-155">Es decir, cuando se acepta un WebSocket, la solicitud deja de avanzar por la canalización, del mismo modo que cuando se alcanza una acción de MVC, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="39d62-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="39d62-156">Pero cuando se termina este bucle y se cierra el socket, la solicitud vuelve a recorrer la canalización.</span><span class="sxs-lookup"><span data-stu-id="39d62-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39d62-157">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="39d62-157">Next steps</span></span>

<span data-ttu-id="39d62-158">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompaña a este artículo es una aplicación de eco sencilla.</span><span class="sxs-lookup"><span data-stu-id="39d62-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="39d62-159">Tiene una página web que realiza conexiones WebSocket y el servidor simplemente reenvía al cliente todos los mensajes que recibe.</span><span class="sxs-lookup"><span data-stu-id="39d62-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="39d62-160">Ejecútela desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y navegue a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="39d62-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="39d62-161">La página web muestra el estado de conexión en la parte superior izquierda:</span><span class="sxs-lookup"><span data-stu-id="39d62-161">The web page shows connection status at the upper left:</span></span>

![Estado inicial de la página web](websockets/_static/start.png)

<span data-ttu-id="39d62-163">Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="39d62-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="39d62-164">Escriba un mensaje de prueba y seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="39d62-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="39d62-165">Cuando haya terminado, seleccione **Close Socket** (Cerrar socket).</span><span class="sxs-lookup"><span data-stu-id="39d62-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="39d62-166">Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.</span><span class="sxs-lookup"><span data-stu-id="39d62-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial de la página web](websockets/_static/end.png)
