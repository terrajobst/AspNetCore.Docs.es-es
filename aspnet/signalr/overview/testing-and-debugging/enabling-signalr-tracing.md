---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitar el seguimiento de SignalR | Documentos de Microsoft
author: tfitzmac
description: Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR. El seguimiento le permite ver información acerca de los eventos de diagnóstico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032823"
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="d89f4-104">Habilitar el seguimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="d89f4-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="d89f4-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d89f4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d89f4-106">Este documento describe cómo habilitar y configurar el seguimiento de los clientes y servidores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d89f4-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="d89f4-107">El seguimiento le permite ver información de diagnóstico acerca de los eventos de la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d89f4-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="d89f4-108">En este tema se escribió originalmente por Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="d89f4-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d89f4-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="d89f4-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="d89f4-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d89f4-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d89f4-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="d89f4-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="d89f4-112">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="d89f4-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d89f4-113">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="d89f4-113">Questions and comments</span></span>
> 
> <span data-ttu-id="d89f4-114">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="d89f4-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d89f4-115">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d89f4-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="d89f4-116">Cuando se habilita el seguimiento, una aplicación de SignalR crea entradas de registro de eventos.</span><span class="sxs-lookup"><span data-stu-id="d89f4-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="d89f4-117">Puede registrar eventos desde el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="d89f4-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="d89f4-118">En la conexión del servidor de registros, proveedor de ampliación y eventos de bus de mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="d89f4-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="d89f4-119">Seguimiento de los eventos de conexión de registros de cliente.</span><span class="sxs-lookup"><span data-stu-id="d89f4-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="d89f4-120">En SignalR 2.1 y versiones posteriores, el seguimiento en el cliente registra todo el contenido de los mensajes de invocación de concentrador.</span><span class="sxs-lookup"><span data-stu-id="d89f4-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="d89f4-121">Contenido</span><span class="sxs-lookup"><span data-stu-id="d89f4-121">Contents</span></span>

- [<span data-ttu-id="d89f4-122">Habilitar el seguimiento en el servidor</span><span class="sxs-lookup"><span data-stu-id="d89f4-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="d89f4-123">Registro de eventos de servidor en archivos de texto</span><span class="sxs-lookup"><span data-stu-id="d89f4-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="d89f4-124">Registro de eventos de servidor en el registro de eventos</span><span class="sxs-lookup"><span data-stu-id="d89f4-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="d89f4-125">Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)</span><span class="sxs-lookup"><span data-stu-id="d89f4-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="d89f4-126">Registro de eventos de cliente de escritorio en la consola de</span><span class="sxs-lookup"><span data-stu-id="d89f4-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="d89f4-127">Registro de eventos de cliente de escritorio en un archivo de texto</span><span class="sxs-lookup"><span data-stu-id="d89f4-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="d89f4-128">Habilitar el seguimiento en los clientes de Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="d89f4-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="d89f4-129">Registro de eventos de cliente de Windows Phone en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="d89f4-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="d89f4-130">Registro de eventos de cliente de Windows Phone en la consola de depuración</span><span class="sxs-lookup"><span data-stu-id="d89f4-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="d89f4-131">Habilitar el seguimiento en el cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d89f4-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="d89f4-132">Habilitar el seguimiento en el servidor</span><span class="sxs-lookup"><span data-stu-id="d89f4-132">Enabling tracing on the server</span></span>

<span data-ttu-id="d89f4-133">Se habilita el seguimiento en el servidor en el archivo de configuración de la aplicación (App.config o Web.config según el tipo de proyecto.) Especificar qué categorías de eventos que desee registrar.</span><span class="sxs-lookup"><span data-stu-id="d89f4-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="d89f4-134">En el archivo de configuración, también especificar si se debe registrar los eventos en un archivo de texto, el registro de eventos de Windows o un registro personalizado mediante la implementación de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="d89f4-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="d89f4-135">Las categorías de eventos de servidor incluyen a los siguientes tipos de mensajes:</span><span class="sxs-lookup"><span data-stu-id="d89f4-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="d89f4-136">Origen</span><span class="sxs-lookup"><span data-stu-id="d89f4-136">Source</span></span> | <span data-ttu-id="d89f4-137">Mensajes</span><span class="sxs-lookup"><span data-stu-id="d89f4-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="d89f4-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="d89f4-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="d89f4-139">El programa de instalación del proveedor de ampliación de Bus de mensajes de SQL, la operación de base de datos, errores y eventos de tiempo de espera</span><span class="sxs-lookup"><span data-stu-id="d89f4-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="d89f4-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="d89f4-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="d89f4-141">Creación de tema de proveedor de ampliación de bus de servicio y suscripción, errores y eventos de mensajería</span><span class="sxs-lookup"><span data-stu-id="d89f4-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="d89f4-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="d89f4-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="d89f4-143">Eventos de conexión, desconexión y error de proveedor de ampliación de Redis</span><span class="sxs-lookup"><span data-stu-id="d89f4-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="d89f4-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="d89f4-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="d89f4-145">Eventos de mensajería de ampliación</span><span class="sxs-lookup"><span data-stu-id="d89f4-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="d89f4-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="d89f4-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="d89f4-147">Eventos de conexión, desconexión, mensajería y error de transporte de WebSocket</span><span class="sxs-lookup"><span data-stu-id="d89f4-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="d89f4-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="d89f4-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="d89f4-149">Eventos de conexión, desconexión, mensajería y error de transporte ServerSentEvents</span><span class="sxs-lookup"><span data-stu-id="d89f4-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="d89f4-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="d89f4-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="d89f4-151">Eventos de conexión, desconexión, mensajería y error de transporte ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="d89f4-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="d89f4-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="d89f4-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="d89f4-153">Eventos de conexión, desconexión, mensajería y error de transporte LongPolling</span><span class="sxs-lookup"><span data-stu-id="d89f4-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="d89f4-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="d89f4-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="d89f4-155">Eventos de conexión y desconexión, keepalive de transporte</span><span class="sxs-lookup"><span data-stu-id="d89f4-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="d89f4-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="d89f4-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="d89f4-157">Eventos de detección de base de datos central</span><span class="sxs-lookup"><span data-stu-id="d89f4-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="d89f4-158">Registro de eventos de servidor en archivos de texto</span><span class="sxs-lookup"><span data-stu-id="d89f4-158">Logging server events to text files</span></span>

<span data-ttu-id="d89f4-159">El código siguiente muestra cómo habilitar el seguimiento para cada categoría de eventos.</span><span class="sxs-lookup"><span data-stu-id="d89f4-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="d89f4-160">Este ejemplo configura la aplicación para registrar eventos en archivos de texto.</span><span class="sxs-lookup"><span data-stu-id="d89f4-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="d89f4-161">**Código de servidor XML para habilitar el seguimiento**</span><span class="sxs-lookup"><span data-stu-id="d89f4-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="d89f4-162">En el código anterior, el `SignalRSwitch` entrada especifica la [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilizados para los eventos enviados en el registro especificado.</span><span class="sxs-lookup"><span data-stu-id="d89f4-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="d89f4-163">En este caso, se establece `Verbose` lo que significa que todos los de depuración y seguimiento de mensajes se registran.</span><span class="sxs-lookup"><span data-stu-id="d89f4-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="d89f4-164">La siguiente salida muestra las entradas de la `transports.log.txt` archivo para una aplicación con el archivo de configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="d89f4-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="d89f4-165">Muestra una nueva conexión, una conexión quitada y eventos de latido del transporte.</span><span class="sxs-lookup"><span data-stu-id="d89f4-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="d89f4-166">Registro de eventos de servidor en el registro de eventos</span><span class="sxs-lookup"><span data-stu-id="d89f4-166">Logging server events to the event log</span></span>

<span data-ttu-id="d89f4-167">Para registrar eventos en el registro de eventos en lugar de un archivo de texto, cambiar los valores de las entradas de la `sharedListeners` nodo.</span><span class="sxs-lookup"><span data-stu-id="d89f4-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="d89f4-168">El código siguiente muestra cómo registrar eventos de servidor en el registro de eventos:</span><span class="sxs-lookup"><span data-stu-id="d89f4-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="d89f4-169">**Código de servidor XML para registrar eventos en el registro de eventos**</span><span class="sxs-lookup"><span data-stu-id="d89f4-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="d89f4-170">Los eventos se registran en el registro de aplicación y están disponibles mediante el Visor de eventos, tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="d89f4-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Visor de eventos que muestra registros de SignalR](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="d89f4-172">Al utilizar el registro de eventos, establezca la **TraceLevel** a **Error** para mantener el número de mensajes fáciles de administrar.</span><span class="sxs-lookup"><span data-stu-id="d89f4-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="d89f4-173">Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)</span><span class="sxs-lookup"><span data-stu-id="d89f4-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="d89f4-174">El cliente .NET puede registrar eventos en la consola, un archivo de texto, o en un registro personalizado mediante la implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="d89f4-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="d89f4-175">Para habilitar el registro en el cliente. NET, establezca la conexión `TraceLevel` propiedad a una [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) valor y el `TraceWriter` propiedad válido [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instancia.</span><span class="sxs-lookup"><span data-stu-id="d89f4-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="d89f4-176">Registro de eventos de cliente de escritorio en la consola de</span><span class="sxs-lookup"><span data-stu-id="d89f4-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="d89f4-177">El siguiente código de C# muestra cómo registrar los eventos en el cliente de .NET en la consola:</span><span class="sxs-lookup"><span data-stu-id="d89f4-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="d89f4-178">Registro de eventos de cliente de escritorio en un archivo de texto</span><span class="sxs-lookup"><span data-stu-id="d89f4-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="d89f4-179">El siguiente código de C# muestra cómo registrar los eventos en el cliente de .NET en un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="d89f4-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="d89f4-180">La siguiente salida muestra las entradas de la `ClientLog.txt` archivo para una aplicación con el archivo de configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="d89f4-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="d89f4-181">Muestra el cliente que se conecta al servidor y el concentrador de invocar un método de cliente denominado `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="d89f4-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="d89f4-182">Habilitar el seguimiento en los clientes de Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="d89f4-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="d89f4-183">Aplicaciones de SignalR para las aplicaciones de Windows Phone utilizan el mismo cliente de .NET como aplicaciones de escritorio, pero [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) y escribir en un archivo con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="d89f4-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="d89f4-184">En su lugar, debe crear una implementación personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="d89f4-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="d89f4-185">Registro de eventos de cliente de Windows Phone en la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="d89f4-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="d89f4-186">El [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) incluye un ejemplo de Windows Phone que escribe los resultados del seguimiento a un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando un comparador [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementación llama `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="d89f4-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="d89f4-187">Esta clase puede encontrarse en el **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** proyecto.</span><span class="sxs-lookup"><span data-stu-id="d89f4-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="d89f4-188">Al crear una instancia de `TextBlockWriter`, pasar actual [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)y un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) donde va a crear un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) desea usar para seguimiento resultado:</span><span class="sxs-lookup"><span data-stu-id="d89f4-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="d89f4-189">El resultado del seguimiento, a continuación, se escribirá en un nuevo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creado en el [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) se pasó en:</span><span class="sxs-lookup"><span data-stu-id="d89f4-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="d89f4-190">Registro de eventos de cliente de Windows Phone en la consola de depuración</span><span class="sxs-lookup"><span data-stu-id="d89f4-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="d89f4-191">Para enviar la salida a la consola de depuración, en lugar de la interfaz de usuario, cree una implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que escribe en la ventana de depuración y asignarlo a la conexión [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) propiedad:</span><span class="sxs-lookup"><span data-stu-id="d89f4-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="d89f4-192">Información de seguimiento, a continuación, se escribirán en la ventana de depuración en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d89f4-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="d89f4-193">Habilitar el seguimiento en el cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d89f4-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="d89f4-194">Para habilitar el registro de cliente en una conexión, establecer el `logging` propiedad en el objeto de conexión antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="d89f4-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="d89f4-195">**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="d89f4-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="d89f4-196">**Código de JavaScript de cliente para habilitar el seguimiento de la consola del explorador (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="d89f4-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="d89f4-197">Cuando el seguimiento está habilitado, el cliente de JavaScript registra eventos en la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="d89f4-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="d89f4-198">Para obtener acceso a la consola del explorador, consulte [supervisión transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="d89f4-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="d89f4-199">Captura de pantalla siguiente muestra a un cliente de SignalR JavaScript con el seguimiento habilitado.</span><span class="sxs-lookup"><span data-stu-id="d89f4-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="d89f4-200">Muestra eventos de invocación de concentrador y de conexión en la consola del explorador:</span><span class="sxs-lookup"><span data-stu-id="d89f4-200">It shows connection and hub invocation events in the browser console:</span></span>

![Eventos de seguimiento de SignalR en la consola del explorador](enabling-signalr-tracing/_static/image4.png)
