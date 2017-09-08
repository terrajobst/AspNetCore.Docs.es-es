---
title: Compatibilidad con WebSockets en ASP.NET Core
author: tdykstra
description: "¿Qué es WebSockets se admiten en ASP.NET Core y cómo utilizarlo."
keywords: "Núcleo de ASP.NET, WebSockets"
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: daa36d117749868e4bb9311a941b7751d7b64744
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="eb2e2-104">Introducción a WebSockets en el núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eb2e2-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="eb2e2-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-enfermera](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="eb2e2-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="eb2e2-106">Este artículo explica cómo empezar a usar WebSockets en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="eb2e2-107">[WebSocket](https://en.wikipedia.org/wiki/WebSocket) es un protocolo que permite a los canales de comunicación bidireccional de persistente a través de las conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-107">[WebSocket](https://en.wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="eb2e2-108">Se utiliza para las aplicaciones como chat, tableros de cotizaciones, juegos, en cualquier lugar desea utilizar las funciones en tiempo real en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="eb2e2-109">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span><span class="sxs-lookup"><span data-stu-id="eb2e2-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample).</span></span> <span data-ttu-id="eb2e2-110">Consulte la [pasos](#next-steps) sección para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="eb2e2-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="eb2e2-111">Prerequisites</span></span>

* <span data-ttu-id="eb2e2-112">Núcleo de ASP.NET 1.1 (no se ejecuta en 1.0)</span><span class="sxs-lookup"><span data-stu-id="eb2e2-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="eb2e2-113">Cualquier sistema operativo que se ejecuta ASP.NET Core en:</span><span class="sxs-lookup"><span data-stu-id="eb2e2-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="eb2e2-114">Windows 7 / Windows Server 2008 y versiones posterior</span><span class="sxs-lookup"><span data-stu-id="eb2e2-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="eb2e2-115">Linux</span><span class="sxs-lookup"><span data-stu-id="eb2e2-115">Linux</span></span>
  * <span data-ttu-id="eb2e2-116">macOS</span><span class="sxs-lookup"><span data-stu-id="eb2e2-116">macOS</span></span>

* <span data-ttu-id="eb2e2-117">**Excepción**: si la aplicación se ejecuta en Windows con IIS, o con WebListener, debe utilizar:</span><span class="sxs-lookup"><span data-stu-id="eb2e2-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="eb2e2-118">Windows 8 / Windows Server 2012 o posterior</span><span class="sxs-lookup"><span data-stu-id="eb2e2-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="eb2e2-119">IIS 8 / Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="eb2e2-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="eb2e2-120">WebSocket debe estar habilitada en IIS</span><span class="sxs-lookup"><span data-stu-id="eb2e2-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="eb2e2-121">Para los exploradores admitidos, consulte http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="eb2e2-122">Cuándo usarlo</span><span class="sxs-lookup"><span data-stu-id="eb2e2-122">When to use it</span></span>

<span data-ttu-id="eb2e2-123">Usar WebSockets cuando necesite trabajar directamente con una conexión de socket.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="eb2e2-124">Por ejemplo, quizá necesite el mejor rendimiento posible para un juego en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="eb2e2-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) proporciona una experiencia mejor modelo de aplicación para la funcionalidad en tiempo real, pero sólo se ejecuta en ASP.NET, no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="eb2e2-126">Una versión de SignalR Core está en desarrollo; Para seguir su progreso, vea el [repositorio de GitHub para SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="eb2e2-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="eb2e2-127">Si no desea esperar SignalR Core, ahora puede usar WebSockets directamente.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="eb2e2-128">Pero es posible que deba desarrollar características que se proporciona SignalR, como:</span><span class="sxs-lookup"><span data-stu-id="eb2e2-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="eb2e2-129">Compatibilidad con una gama más amplia de versiones del explorador utilizando la reserva automática a los métodos de transporte alternativo.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="eb2e2-130">Reconexión automática cuando un fallo de conexión.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="eb2e2-131">Compatibilidad con clientes que llaman a métodos en el servidor o viceversa.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="eb2e2-132">Soporte técnico para el escalado a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="eb2e2-133">Cómo se usa</span><span class="sxs-lookup"><span data-stu-id="eb2e2-133">How to use it</span></span>

* <span data-ttu-id="eb2e2-134">Instalar el [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paquete.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="eb2e2-135">Configurar el middleware.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-135">Configure the middleware.</span></span>
* <span data-ttu-id="eb2e2-136">Aceptar las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="eb2e2-137">Enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="eb2e2-138">Configurar el middleware</span><span class="sxs-lookup"><span data-stu-id="eb2e2-138">Configure the middleware</span></span>

<span data-ttu-id="eb2e2-139">Agregar el middleware de WebSockets en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="eb2e2-140">Se pueden configurar las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="eb2e2-140">The following settings can be configured:</span></span>

* <span data-ttu-id="eb2e2-141">`KeepAliveInterval`-La frecuencia con que enviar tramas de "ping" al cliente, para asegurarse de servidores proxy mantener abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="eb2e2-142">`ReceiveBufferSize`-El tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="eb2e2-143">Sólo los usuarios avanzados necesitaría cambiarlo, para la optimización de rendimiento en función del tamaño de sus datos.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="eb2e2-144">Acepte las solicitudes WebSocket</span><span class="sxs-lookup"><span data-stu-id="eb2e2-144">Accept WebSocket requests</span></span>

<span data-ttu-id="eb2e2-145">En algún lugar más adelante en el ciclo de vida de la solicitud (más adelante en el `Configure` método o en una acción de MVC, por ejemplo) Compruebe si es una solicitud de WebSocket y aceptar la solicitud de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="eb2e2-146">Este ejemplo es de más adelante en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="eb2e2-147">Una solicitud de WebSocket puede proceder cualquier dirección URL, pero este código de ejemplo solo acepta las solicitudes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="eb2e2-148">Enviar y recibir mensajes</span><span class="sxs-lookup"><span data-stu-id="eb2e2-148">Send and receive messages</span></span>

<span data-ttu-id="eb2e2-149">El `AcceptWebSocketAsync` método actualiza la conexión TCP a una conexión de WebSocket y le ofrece una [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) objeto.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="eb2e2-150">Utilizar el objeto de WebSocket para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="eb2e2-151">El código mostrado anteriormente que acepta la solicitud de WebSocket pasa el `WebSocket` el objeto a una `Echo` método; este es el `Echo` (método).</span><span class="sxs-lookup"><span data-stu-id="eb2e2-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="eb2e2-152">El código recibe un mensaje y devuelve inmediatamente el mismo mensaje.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="eb2e2-153">Se mantiene en un bucle realice hasta que el cliente cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="eb2e2-154">Cuando se acepta el socket Web antes de comenzar este bucle, se finaliza la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="eb2e2-155">Tras cerrar el socket, se desenreda la canalización.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="eb2e2-156">Es decir, se detiene el solicitud avanzando en la canalización al aceptar un WebSocket, al igual que lo haría cuando se alcanza una acción de MVC, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="eb2e2-157">Pero cuando haya terminado este bucle y cierra el socket, la solicitud se realizará copia de seguridad de la canalización.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb2e2-158">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="eb2e2-158">Next steps</span></span>

<span data-ttu-id="eb2e2-159">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que muestra en este artículo es una aplicación de repetición simple.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="eb2e2-160">Tiene una página web que realiza las conexiones de WebSocket y el servidor solo vuelve a enviar al cliente todos los mensajes que recibe.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="eb2e2-161">Ejecutar desde un símbolo del sistema (no configuró para ejecutar desde Visual Studio con IIS Express) y vaya a http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="eb2e2-162">La página web muestra el estado de conexión en la parte superior izquierda:</span><span class="sxs-lookup"><span data-stu-id="eb2e2-162">The web page shows connection status at the upper left:</span></span>

![Estado inicial de la página web](websockets/_static/start.png)

<span data-ttu-id="eb2e2-164">Seleccione **conectar** para enviar una solicitud de WebSocket para la dirección URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="eb2e2-165">Escriba un mensaje de prueba y seleccione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="eb2e2-166">Cuando hayas terminado, selecciona **Socket cerrar**.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="eb2e2-167">El **Log comunicación** sección notifica cada apertura, envío y acción al cerrar tal y como ocurre.</span><span class="sxs-lookup"><span data-stu-id="eb2e2-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial de la página web](websockets/_static/end.png)
