---
title: Compatibilidad con WebSockets en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar WebSockets en ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153688"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="81480-103">Compatibilidad con WebSockets en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81480-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="81480-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="81480-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="81480-105">En este artículo se ofrece una introducción a WebSockets en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81480-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="81480-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="81480-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="81480-107">Se usa en aplicaciones que sacan partido de comunicaciones rápidas y en tiempo real, como las aplicaciones de chat, panel y juegos.</span><span class="sxs-lookup"><span data-stu-id="81480-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="81480-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="81480-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="81480-109">Para más información, vea la sección [Pasos siguientes](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="81480-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81480-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="81480-110">Prerequisites</span></span>

* <span data-ttu-id="81480-111">ASP.NET Core 1.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="81480-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="81480-112">Cualquier sistema operativo que admita ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="81480-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="81480-113">Windows 7/Windows Server 2008 o posterior</span><span class="sxs-lookup"><span data-stu-id="81480-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="81480-114">Linux</span><span class="sxs-lookup"><span data-stu-id="81480-114">Linux</span></span>
  * <span data-ttu-id="81480-115">macOS</span><span class="sxs-lookup"><span data-stu-id="81480-115">macOS</span></span>
  
* <span data-ttu-id="81480-116">Si la aplicación se ejecuta en Windows con IIS:</span><span class="sxs-lookup"><span data-stu-id="81480-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="81480-117">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="81480-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="81480-118">IIS 8/Express IIS 8</span><span class="sxs-lookup"><span data-stu-id="81480-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="81480-119">WebSockets debe estar habilitado en IIS (vea la sección [Compatibilidad con IIS/IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="81480-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="81480-120">Si la aplicación se ejecuta en [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="81480-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="81480-121">Windows 8/Windows Server 2012 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="81480-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="81480-122">Para saber qué exploradores son compatibles, vea https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="81480-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="81480-123">Cuándo usar WebSockets</span><span class="sxs-lookup"><span data-stu-id="81480-123">When to use WebSockets</span></span>

<span data-ttu-id="81480-124">Use WebSockets para trabajar directamente con una conexión de socket.</span><span class="sxs-lookup"><span data-stu-id="81480-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="81480-125">Por ejemplo, úselo para lograr el mejor rendimiento posible en un juego en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="81480-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="81480-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) proporciona un modelo de aplicación más completo para funciones en tiempo real, pero solo se ejecuta en ASP.NET 4.x, no en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81480-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="81480-127">Hay una versión de SignalR para ASP.NET Core programada para su lanzamiento con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="81480-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="81480-128">Vea [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288) (Planeación de alto nivel de ASP.NET Core 2.1).</span><span class="sxs-lookup"><span data-stu-id="81480-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="81480-129">Hasta el lanzamiento de SignalR Core, puede usar WebSockets,</span><span class="sxs-lookup"><span data-stu-id="81480-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="81480-130">pero mientras tanto el desarrollador deberá suministrar y admitir las características que SignalR proporciona.</span><span class="sxs-lookup"><span data-stu-id="81480-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="81480-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="81480-131">For example:</span></span>

* <span data-ttu-id="81480-132">Compatibilidad con una gama más amplia de versiones del explorador, ya que usa la reserva automática para los métodos de transporte alternativos.</span><span class="sxs-lookup"><span data-stu-id="81480-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="81480-133">Reconexión automática cuando se produce un fallo de conexión.</span><span class="sxs-lookup"><span data-stu-id="81480-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="81480-134">Compatibilidad con clientes que llaman a métodos en el servidor o viceversa.</span><span class="sxs-lookup"><span data-stu-id="81480-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="81480-135">Compatibilidad con el escalado a varios servidores.</span><span class="sxs-lookup"><span data-stu-id="81480-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="81480-136">Cómo se usa</span><span class="sxs-lookup"><span data-stu-id="81480-136">How to use it</span></span>

* <span data-ttu-id="81480-137">Instale el paquete [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="81480-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="81480-138">Configure el middleware.</span><span class="sxs-lookup"><span data-stu-id="81480-138">Configure the middleware.</span></span>
* <span data-ttu-id="81480-139">Acepte las solicitudes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="81480-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="81480-140">Envíe y reciba mensajes.</span><span class="sxs-lookup"><span data-stu-id="81480-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="81480-141">Configurar el middleware</span><span class="sxs-lookup"><span data-stu-id="81480-141">Configure the middleware</span></span>

<span data-ttu-id="81480-142">Agregue el middleware de WebSockets al método `Configure` de la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="81480-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="81480-143">Se pueden configurar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="81480-143">The following settings can be configured:</span></span>

* <span data-ttu-id="81480-144">`KeepAliveInterval`: la frecuencia con que se envían marcos "ping" al cliente, para asegurarse de que los servidores proxy mantienen abierta la conexión.</span><span class="sxs-lookup"><span data-stu-id="81480-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="81480-145">`ReceiveBufferSize`: el tamaño del búfer usado para recibir datos.</span><span class="sxs-lookup"><span data-stu-id="81480-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="81480-146">Puede que los usuarios avanzados tengan que cambiar estas opciones para ajustar el rendimiento según el tamaño de los datos.</span><span class="sxs-lookup"><span data-stu-id="81480-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="81480-147">Aceptar solicitudes WebSocket</span><span class="sxs-lookup"><span data-stu-id="81480-147">Accept WebSocket requests</span></span>

<span data-ttu-id="81480-148">En algún momento posterior en el ciclo de solicitudes (más adelante en el método `Configure` o en una acción de MVC, por ejemplo) debe comprobar si se trata de una solicitud WebSocket y aceptarla.</span><span class="sxs-lookup"><span data-stu-id="81480-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="81480-149">El siguiente ejemplo se corresponde con un momento más adelante en el método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="81480-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="81480-150">Una solicitud WebSocket puede proceder de cualquier dirección URL, pero este código de ejemplo solo acepta solicitudes de `/ws`.</span><span class="sxs-lookup"><span data-stu-id="81480-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="81480-151">Enviar y recibir mensajes</span><span class="sxs-lookup"><span data-stu-id="81480-151">Send and receive messages</span></span>

<span data-ttu-id="81480-152">El método `AcceptWebSocketAsync` actualiza la conexión TCP a una conexión WebSocket y proporciona un objeto [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="81480-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="81480-153">Use el objeto `WebSocket` para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="81480-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="81480-154">El código antes mostrado que acepta la solicitud WebSocket pasa el objeto `WebSocket` a un método `Echo`.</span><span class="sxs-lookup"><span data-stu-id="81480-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="81480-155">El código recibe un mensaje y devuelve inmediatamente el mismo mensaje.</span><span class="sxs-lookup"><span data-stu-id="81480-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="81480-156">Los mensajes se envían y reciben en un bucle hasta que el cliente cierra la conexión:</span><span class="sxs-lookup"><span data-stu-id="81480-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="81480-157">Cuando la conexión WebSocket se acepta antes de que el bucle comience, la canalización de middleware finaliza.</span><span class="sxs-lookup"><span data-stu-id="81480-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="81480-158">Tras cerrar el socket, se desenreda la canalización.</span><span class="sxs-lookup"><span data-stu-id="81480-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="81480-159">Es decir, la solicitud deja de avanzar en la canalización cuando WebSocket se acepta,</span><span class="sxs-lookup"><span data-stu-id="81480-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="81480-160">pero cuando el bucle termina y el socket se cierra, la solicitud vuelve a recorrer la canalización.</span><span class="sxs-lookup"><span data-stu-id="81480-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="81480-161">Compatibilidad con IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="81480-161">IIS/IIS Express support</span></span>

<span data-ttu-id="81480-162">El protocolo WebSocket se puede usar en Windows Server 2012 o posterior, y en Windows 8 o posterior con IIS o IIS Express 8 o posterior.</span><span class="sxs-lookup"><span data-stu-id="81480-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="81480-163">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 2012 o posterior:</span><span class="sxs-lookup"><span data-stu-id="81480-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="81480-164">Use el asistente **Agregar roles y características** del menú **Administrar** o el vínculo de **Administrador del servidor**.</span><span class="sxs-lookup"><span data-stu-id="81480-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="81480-165">Seleccione **Instalación basada en características o en roles**.</span><span class="sxs-lookup"><span data-stu-id="81480-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="81480-166">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="81480-166">Select **Next**.</span></span>
1. <span data-ttu-id="81480-167">Seleccione el servidor que corresponda (el servidor local está seleccionado de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="81480-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="81480-168">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="81480-168">Select **Next**.</span></span>
1. <span data-ttu-id="81480-169">Expanda **Servidor web (IIS)** en el árbol **Roles**, expanda **Servidor web** y, por último, expanda **Desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="81480-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="81480-170">Seleccione **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="81480-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="81480-171">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="81480-171">Select **Next**.</span></span>
1. <span data-ttu-id="81480-172">Si no necesita más características, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="81480-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="81480-173">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="81480-173">Select **Install**.</span></span>
1. <span data-ttu-id="81480-174">Cuando la instalación finalice, haga clic en **Cerrar** para salir del asistente.</span><span class="sxs-lookup"><span data-stu-id="81480-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="81480-175">Para habilitar la compatibilidad con el protocolo WebSocket en Windows Server 8 o posterior:</span><span class="sxs-lookup"><span data-stu-id="81480-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="81480-176">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="81480-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="81480-177">Abra los siguientes nodos: **Internet Information Services** > **Servicios World Wide Web** > **Características de desarrollo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="81480-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="81480-178">Seleccione la característica **Protocolo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="81480-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="81480-179">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="81480-179">Select **OK**.</span></span>

<span data-ttu-id="81480-180">**Deshabilitar WebSocket al usar socket.io en node.js**</span><span class="sxs-lookup"><span data-stu-id="81480-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="81480-181">Si usa la compatibilidad de WebSocket en [socket.io](https://socket.io/) en [Node.js](https://nodejs.org/), deshabilite el módulo IIS WebSocket predeterminado, usando para ello el elemento `webSocket` de *web.config* o de *applicationHost.config*. Si este paso no se lleva a cabo, el módulo IIS WebSocket intenta controlar la comunicación de WebSocket en lugar de Node.js y la aplicación.</span><span class="sxs-lookup"><span data-stu-id="81480-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="81480-182">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="81480-182">Next steps</span></span>

<span data-ttu-id="81480-183">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) que acompaña a este artículo es una aplicación de eco.</span><span class="sxs-lookup"><span data-stu-id="81480-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="81480-184">Tiene una página web que realiza las conexiones WebSocket y el servidor reenvía de vuelta al cliente todos los mensajes que reciba.</span><span class="sxs-lookup"><span data-stu-id="81480-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="81480-185">Ejecute la aplicación desde un símbolo del sistema (no está configurada para ejecutarse desde Visual Studio con IIS Express) y vaya a http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="81480-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="81480-186">En la página web se muestra el estado de conexión en la parte superior izquierda:</span><span class="sxs-lookup"><span data-stu-id="81480-186">The web page shows the connection status in the upper left:</span></span>

![Estado inicial de la página web](websockets/_static/start.png)

<span data-ttu-id="81480-188">Seleccione **Connect** (Conectar) para enviar una solicitud WebSocket para la URL mostrada.</span><span class="sxs-lookup"><span data-stu-id="81480-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="81480-189">Escriba un mensaje de prueba y seleccione **Send** (Enviar).</span><span class="sxs-lookup"><span data-stu-id="81480-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="81480-190">Cuando haya terminado, seleccione **Close Socket** (Cerrar socket).</span><span class="sxs-lookup"><span data-stu-id="81480-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="81480-191">Los informes de la sección **Communication Log** (Registro de comunicación) informan de cada acción de abrir, enviar y cerrar a medida que se producen.</span><span class="sxs-lookup"><span data-stu-id="81480-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Estado inicial de la página web](websockets/_static/end.png)
