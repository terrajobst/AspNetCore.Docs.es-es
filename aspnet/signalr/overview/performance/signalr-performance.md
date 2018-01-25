---
uid: signalr/overview/performance/signalr-performance
title: Rendimiento de SignalR | Documentos de Microsoft
author: pfletcher
description: Rendimiento de SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4468ee8031afccca847db67bd4b5b263f0a2c5ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance"></a><span data-ttu-id="32c71-103">Rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="32c71-103">SignalR Performance</span></span>
====================
<span data-ttu-id="32c71-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="32c71-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="32c71-105">En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="32c71-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="32c71-106">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="32c71-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="32c71-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="32c71-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="32c71-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="32c71-108">.NET 4.5</span></span>
> - <span data-ttu-id="32c71-109">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="32c71-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="32c71-110">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="32c71-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="32c71-111">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="32c71-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="32c71-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="32c71-112">Questions and comments</span></span>
> 
> <span data-ttu-id="32c71-113">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="32c71-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="32c71-114">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="32c71-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="32c71-115">Para ver una presentación reciente sobre el rendimiento de SignalR y ajuste de escala, vea [ajuste de escala en la Web en tiempo real mediante ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="32c71-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="32c71-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="32c71-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="32c71-117">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="32c71-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="32c71-118">Optimización del servidor de SignalR para el rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="32c71-119">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="32c71-120">Uso de contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="32c71-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="32c71-121">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="32c71-122">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="32c71-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="32c71-123">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="32c71-123">Design considerations</span></span>

<span data-ttu-id="32c71-124">Esta sección describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que no se se impide el rendimiento mediante la generación de tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="32c71-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="32c71-125">Límite de frecuencia de mensajes</span><span class="sxs-lookup"><span data-stu-id="32c71-125">Throttling message frequency</span></span>

<span data-ttu-id="32c71-126">Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no es necesario enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="32c71-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="32c71-127">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que las colas y los envía no mensajes que supere con frecuencia una tarifa fija (es decir, hasta un número determinado de mensajes se enviará cada segundo, si hay mensajes en ese momento en terval para enviarse).</span><span class="sxs-lookup"><span data-stu-id="32c71-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="32c71-128">Para una aplicación de ejemplo que limita los mensajes a una velocidad de cierta (desde el cliente y servidor), consulte [alta frecuencia en tiempo real con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="32c71-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="32c71-129">Reducir el tamaño del mensaje</span><span class="sxs-lookup"><span data-stu-id="32c71-129">Reducing message size</span></span>

<span data-ttu-id="32c71-130">Puede reducir el tamaño de un mensaje de SignalR al reducir el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="32c71-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="32c71-131">En el código de servidor, si va a enviar un objeto que contiene propiedades que no es necesario para que se transmitan, impedir que las propiedades que se va a serializar mediante el uso del `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="32c71-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="32c71-132">Los nombres de propiedades también se almacenan en el mensaje. se puede abreviar los nombres de propiedades con el `JsonProperty` atributo.</span><span class="sxs-lookup"><span data-stu-id="32c71-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="32c71-133">El ejemplo de código siguiente muestra cómo excluir una propiedad de la que se envían al cliente y cómo acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="32c71-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="32c71-134">**Código de servidor de .NET que muestra el atributo JsonIgnore para excluir los datos se envíen al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="32c71-135">Para conservar la legibilidad / mantenimiento en el código de cliente, los nombres de propiedad abreviada pueden ser nombres reasignados a humanos sencillo cuando se recibe el mensaje.</span><span class="sxs-lookup"><span data-stu-id="32c71-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="32c71-136">El ejemplo de código siguiente muestra una posible manera de volver a asignar nombres abreviados a más largos, mediante la definición de un contrato de mensaje (asignación) y el uso de la `reMap` función que se aplica el contrato a la clase optimizada de mensajes:</span><span class="sxs-lookup"><span data-stu-id="32c71-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="32c71-137">**Código de JavaScript del lado cliente que reasigna acorta los nombres de propiedad a los nombres de lenguaje natural**</span><span class="sxs-lookup"><span data-stu-id="32c71-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="32c71-138">Los nombres se puede abreviar en los mensajes desde el cliente al servidor, con el mismo método.</span><span class="sxs-lookup"><span data-stu-id="32c71-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="32c71-139">Reduce la superficie de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del mensaje de objeto también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="32c71-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="32c71-140">Por ejemplo, si el intervalo completo de un `int` no es necesaria, un `short` o `byte` pueden utilizarse en su lugar.</span><span class="sxs-lookup"><span data-stu-id="32c71-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="32c71-141">Puesto que los mensajes se almacenan en el bus de mensajes en memoria del servidor, lo que reduce el tamaño de mensajes también puede solucionar problemas de memoria de servidor.</span><span class="sxs-lookup"><span data-stu-id="32c71-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="32c71-142">Optimización del servidor de SignalR para el rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="32c71-143">Las siguientes opciones de configuración se pueden utilizar para optimizar el servidor para mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="32c71-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="32c71-144">Para obtener información general sobre cómo mejorar el rendimiento de una aplicación ASP.NET, vea [mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="32c71-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="32c71-145">**Opciones de configuración de SignalR**</span><span class="sxs-lookup"><span data-stu-id="32c71-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="32c71-146">**DefaultMessageBufferSize**: de forma predeterminada, SignalR conserva 1000 mensajes en memoria por centro de cada conexión.</span><span class="sxs-lookup"><span data-stu-id="32c71-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="32c71-147">Si se utilizan mensajes de gran tamaño, esto puede crear problemas de memoria que se pueden mitigar si reduce este valor.</span><span class="sxs-lookup"><span data-stu-id="32c71-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="32c71-148">Esta configuración se puede establecer el `Application_Start` controlador de eventos en una aplicación ASP.NET, o en la `Configuration` método de una clase de inicio OWIN en una aplicación hospeda a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="32c71-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="32c71-149">El ejemplo siguiente muestra cómo reducir este valor con el fin de reducir la superficie de memoria de la aplicación con el fin de reducir la cantidad de memoria de servidor usada:</span><span class="sxs-lookup"><span data-stu-id="32c71-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="32c71-150">**Código de servidor de .NET de Startup.cs para reducir el tamaño de búfer de mensaje predeterminado**</span><span class="sxs-lookup"><span data-stu-id="32c71-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="32c71-151">**Opciones de configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="32c71-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="32c71-152">**Máximo de solicitudes simultáneas por aplicación**: aumentar el número de IIS simultáneas solicitudes aumentará disponible para atender las solicitudes de recursos de servidor.</span><span class="sxs-lookup"><span data-stu-id="32c71-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="32c71-153">El valor predeterminado es 5000; Para aumentar este valor, ejecute los comandos siguientes en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="32c71-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="32c71-154">**ApplicationPool QueueLength**: este es el número máximo de solicitudes que Http.sys pone en cola para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="32c71-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="32c71-155">Cuando la cola está llena, las nuevas solicitudes reciben una respuesta 503 "Servicio no disponible".</span><span class="sxs-lookup"><span data-stu-id="32c71-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="32c71-156">El valor predeterminado es 1000.</span><span class="sxs-lookup"><span data-stu-id="32c71-156">The default value is 1000.</span></span>

    <span data-ttu-id="32c71-157">Acortar la longitud de cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación conservar los recursos de memoria.</span><span class="sxs-lookup"><span data-stu-id="32c71-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="32c71-158">Para obtener más información, consulte [administrar, ajuste y configuración de grupos de aplicaciones](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="32c71-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="32c71-159">**Opciones de configuración de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="32c71-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="32c71-160">Esta sección incluye los valores de configuración que se pueden establecer en el `aspnet.config` archivo.</span><span class="sxs-lookup"><span data-stu-id="32c71-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="32c71-161">Este archivo se encuentra en una de estas dos ubicaciones, dependiendo de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="32c71-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="32c71-162">Configuración de ASP.NET que puede mejorar el rendimiento de SignalR incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="32c71-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="32c71-163">**Número máximo de solicitudes simultáneo por CPU**: aumento de esta configuración puede mitigar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="32c71-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="32c71-164">Para aumentar esta opción, agregue la siguiente opción de configuración para el `aspnet.config` archivo:</span><span class="sxs-lookup"><span data-stu-id="32c71-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="32c71-165">**Límite de la cola de peticiones**: cuando se supera el número total de conexiones la `maxConcurrentRequestsPerCPU` configuración, ASP.NET comenzará la limitación de solicitudes mediante una cola.</span><span class="sxs-lookup"><span data-stu-id="32c71-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="32c71-166">Para aumentar el tamaño de la cola, puede aumentar la `requestQueueLimit` configuración.</span><span class="sxs-lookup"><span data-stu-id="32c71-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="32c71-167">Para ello, agregue la siguiente opción de configuración para el `processModel` nodo `config/machine.config` (en lugar de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="32c71-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="32c71-168">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="32c71-169">Esta sección describe las formas de buscar los cuellos de botella de rendimiento en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32c71-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="32c71-170">Comprobar que se está usando WebSocket</span><span class="sxs-lookup"><span data-stu-id="32c71-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="32c71-171">Aunque SignalR puede usar una serie de transportes para la comunicación entre cliente y servidor, WebSocket ofrece una ventaja de rendimiento importante y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="32c71-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="32c71-172">Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="32c71-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="32c71-173">Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollo de explorador y examine los registros para ver qué transporte se utiliza para la conexión.</span><span class="sxs-lookup"><span data-stu-id="32c71-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="32c71-174">Para obtener información sobre el uso de las herramientas de desarrollo de explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="32c71-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="32c71-175">Uso de contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="32c71-175">Using SignalR performance counters</span></span>

<span data-ttu-id="32c71-176">Esta sección describe cómo habilitar y utilizar contadores de rendimiento de SignalR, se encuentra en la `Microsoft.AspNet.SignalR.Utils` paquete.</span><span class="sxs-lookup"><span data-stu-id="32c71-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="32c71-177">Instalar signalr.exe</span><span class="sxs-lookup"><span data-stu-id="32c71-177">Installing signalr.exe</span></span>

<span data-ttu-id="32c71-178">Contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="32c71-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="32c71-179">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="32c71-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="32c71-180">En la aplicación de Visual Studio, seleccione **herramientas**, **Administrador de paquetes de biblioteca**, **administrar paquetes de NuGet para la solución...**</span><span class="sxs-lookup"><span data-stu-id="32c71-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="32c71-181">Busque **signalr.utils**y seleccione instalar.</span><span class="sxs-lookup"><span data-stu-id="32c71-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="32c71-182">Acepte el contrato de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="32c71-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="32c71-183">SignalR.exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="32c71-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="32c71-184">Instalando los contadores de rendimiento con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="32c71-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="32c71-185">Para instalar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="32c71-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="32c71-186">Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="32c71-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="32c71-187">Contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="32c71-187">SignalR Performance counters</span></span>

<span data-ttu-id="32c71-188">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="32c71-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="32c71-189">Los contadores de "Total" medir el número de eventos desde el último grupo de aplicaciones o el servidor se reinicie.</span><span class="sxs-lookup"><span data-stu-id="32c71-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="32c71-190">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="32c71-190">**Connection metrics**</span></span>

<span data-ttu-id="32c71-191">Las siguientes métricas de medir los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="32c71-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="32c71-192">Para obtener más información, consulte [descripción y control de eventos de duración de conexión](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="32c71-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="32c71-193">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="32c71-193">**Connections Connected**</span></span>
- <span data-ttu-id="32c71-194">**Conexiones a conectar**</span><span class="sxs-lookup"><span data-stu-id="32c71-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="32c71-195">**Conexiones desconectado**</span><span class="sxs-lookup"><span data-stu-id="32c71-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="32c71-196">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="32c71-196">**Connections Current**</span></span>

<span data-ttu-id="32c71-197">**Medidas de mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-197">**Message metrics**</span></span>

<span data-ttu-id="32c71-198">Las siguientes métricas de medir el tráfico de mensajes generado por SignalR.</span><span class="sxs-lookup"><span data-stu-id="32c71-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="32c71-199">**Total de mensajes de conexión**</span><span class="sxs-lookup"><span data-stu-id="32c71-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="32c71-200">**Total de mensajes de conexión enviados**</span><span class="sxs-lookup"><span data-stu-id="32c71-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="32c71-201">**Conexión de mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="32c71-202">**Conexión de mensajes enviados/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="32c71-203">**Métricas de bus de mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-203">**Message bus metrics**</span></span>

<span data-ttu-id="32c71-204">Las siguientes métricas de medir el tráfico a través del bus de mensajes de SignalR interno, la cola en el que se colocan todos los mensajes de SignalR entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="32c71-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="32c71-205">Un mensaje es **publicada** cuando se envían o difusión.</span><span class="sxs-lookup"><span data-stu-id="32c71-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="32c71-206">A **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto es igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="32c71-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="32c71-207">Un **trabajo asignado** es un componente que envía datos a las conexiones activas; una **trabajo ocupado** es aquella que está enviando activamente un mensaje.</span><span class="sxs-lookup"><span data-stu-id="32c71-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="32c71-208">**Recibido Total de mensajes de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="32c71-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="32c71-209">**Bus de mensajes mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="32c71-210">**Bus de mensajes de mensajes publican Total**</span><span class="sxs-lookup"><span data-stu-id="32c71-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="32c71-211">**Bus de mensajes de mensajes publicados/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="32c71-212">**Actual de suscriptores de Bus de mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="32c71-213">**Total de suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="32c71-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="32c71-214">**Los suscriptores de Bus de mensajes/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="32c71-215">**Asignada los trabajadores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="32c71-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="32c71-216">**Trabajadores ocupados del Bus de mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="32c71-217">**Actual de temas del Bus de mensaje**</span><span class="sxs-lookup"><span data-stu-id="32c71-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="32c71-218">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="32c71-218">**Error metrics**</span></span>

<span data-ttu-id="32c71-219">Las siguientes métricas de medir los errores generados por el tráfico de mensajes de SignalR.</span><span class="sxs-lookup"><span data-stu-id="32c71-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="32c71-220">**Resolución de concentrador** errores se producen cuando un concentrador o un método de concentrador no se puede resolver.</span><span class="sxs-lookup"><span data-stu-id="32c71-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="32c71-221">**Invocación de concentrador** errores son las excepciones producidas al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="32c71-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="32c71-222">**Transporte** los errores son errores de conexión que se produce durante una solicitud o respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="32c71-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="32c71-223">**Errores: Total de todos los**</span><span class="sxs-lookup"><span data-stu-id="32c71-223">**Errors: All Total**</span></span>
- <span data-ttu-id="32c71-224">**Errores: All/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="32c71-225">**Errores: Total de resolución de concentrador**</span><span class="sxs-lookup"><span data-stu-id="32c71-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="32c71-226">**Errores: Resolución de concentrador / seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="32c71-227">**Errores: Total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="32c71-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="32c71-228">**Errores: Invocación de concentrador / seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="32c71-229">**Errores: Total de transporte**</span><span class="sxs-lookup"><span data-stu-id="32c71-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="32c71-230">**Errores: Transporte/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="32c71-231">**Métricas de ampliación**</span><span class="sxs-lookup"><span data-stu-id="32c71-231">**Scaleout metrics**</span></span>

<span data-ttu-id="32c71-232">Las siguientes métricas de medir el tráfico y los errores generados por el proveedor de ampliación.</span><span class="sxs-lookup"><span data-stu-id="32c71-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="32c71-233">A **flujo** en este contexto es una unidad de escala utilizada por el proveedor de ampliación; se trata de una tabla si se utiliza SQL Server, un tema si se usa el Bus de servicio y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="32c71-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="32c71-234">Cada flujo garantiza ordenada operaciones de lectura y escritura; una única secuencia es un cuello de botella potencial de escala, por lo que puede aumentar el número de secuencias para ayudar a reducir un cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="32c71-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="32c71-235">Si se utilizan varios flujos, SignalR distribuirá automáticamente mensajes (particiones) a través de estas secuencias de forma que garantiza que los mensajes enviados desde cada conexión están en orden.</span><span class="sxs-lookup"><span data-stu-id="32c71-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="32c71-236">El [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) opción controla la longitud de la cola de envío de ampliación mantenida por SignalR.</span><span class="sxs-lookup"><span data-stu-id="32c71-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="32c71-237">Si se establece en un valor mayor que 0 colocará todos los mensajes en una cola de envío para enviar uno a uno al backplane de mensajería configurado.</span><span class="sxs-lookup"><span data-stu-id="32c71-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="32c71-238">Si el tamaño de la cola supera la longitud configurada, las llamadas posteriores al envío tendrán un error inmediatamente con un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes en la cola es menor que el valor nuevo.</span><span class="sxs-lookup"><span data-stu-id="32c71-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="32c71-239">Puesta en cola está deshabilitada de forma predeterminada porque los planos posteriores implementados suelen tengan su propio control de flujo o de puesta en cola en su lugar.</span><span class="sxs-lookup"><span data-stu-id="32c71-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="32c71-240">En el caso de SQL Server, la agrupación de conexiones eficazmente limita el número de envíos en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="32c71-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="32c71-241">De forma predeterminada, solo un flujo se utiliza para SQL Server y Redis, cinco flujos se usan para el Bus de servicio y se deshabilita la puesta en cola, pero esta configuración puede cambiarse a través de la configuración de SQL Server y Service Bus:</span><span class="sxs-lookup"><span data-stu-id="32c71-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="32c71-242">**Código de servidor de .NET para configurar la longitud de cola y el recuento de tabla para backplane de SQL Server**</span><span class="sxs-lookup"><span data-stu-id="32c71-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="32c71-243">**Código de servidor de .NET para configurar la longitud de cola y el recuento de tema de backplane de Bus de servicio**</span><span class="sxs-lookup"><span data-stu-id="32c71-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="32c71-244">A **Buffering** flujo es aquel que ha entrado en un estado de error; cuando la secuencia está en estado de error, todos los mensajes enviados al backplane se producirá un error hasta que ya no es un error en la secuencia.</span><span class="sxs-lookup"><span data-stu-id="32c71-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="32c71-245">El **longitud de cola de envío** es el número de mensajes que se han registrado pero aún no se ha enviado.</span><span class="sxs-lookup"><span data-stu-id="32c71-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="32c71-246">**Bus de mensajes de ampliación mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="32c71-247">**Total de secuencias de ampliación**</span><span class="sxs-lookup"><span data-stu-id="32c71-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="32c71-248">**Ampliación transmite por secuencias abierto**</span><span class="sxs-lookup"><span data-stu-id="32c71-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="32c71-249">**Almacenamiento en búfer de secuencias de ampliación**</span><span class="sxs-lookup"><span data-stu-id="32c71-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="32c71-250">**Total de errores de ampliación**</span><span class="sxs-lookup"><span data-stu-id="32c71-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="32c71-251">**Errores de ampliación/seg.**</span><span class="sxs-lookup"><span data-stu-id="32c71-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="32c71-252">**Longitud de la cola de envío de ampliación**</span><span class="sxs-lookup"><span data-stu-id="32c71-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="32c71-253">Para obtener más información sobre lo que va a medir estos contadores, vea [SignalR Scaleout con Bus de servicio de Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="32c71-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="32c71-254">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="32c71-254">Using other performance counters</span></span>

<span data-ttu-id="32c71-255">Los siguientes contadores de rendimiento también pueden resultarle útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32c71-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="32c71-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="32c71-256">**Memory**</span></span>

- <span data-ttu-id="32c71-257">Memoria de .NET CLR\\número de bytes en todos los montones (por w3wp)</span><span class="sxs-lookup"><span data-stu-id="32c71-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="32c71-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="32c71-258">**ASP.NET**</span></span>

- <span data-ttu-id="32c71-259">ASP. NET\Solicitudes actuales</span><span class="sxs-lookup"><span data-stu-id="32c71-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="32c71-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="32c71-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="32c71-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="32c71-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="32c71-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="32c71-262">**CPU**</span></span>

- <span data-ttu-id="32c71-263">Tiempo de procesador Information\Processor</span><span class="sxs-lookup"><span data-stu-id="32c71-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="32c71-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="32c71-264">**TCP/IP**</span></span>

- <span data-ttu-id="32c71-265">TCPv6/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="32c71-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="32c71-266">TCPv4/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="32c71-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="32c71-267">**Servicio Web**</span><span class="sxs-lookup"><span data-stu-id="32c71-267">**Web Service**</span></span>

- <span data-ttu-id="32c71-268">Web\conexiones</span><span class="sxs-lookup"><span data-stu-id="32c71-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="32c71-269">Conexiones de Service\Maximum Web</span><span class="sxs-lookup"><span data-stu-id="32c71-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="32c71-270">**Subprocesamiento**</span><span class="sxs-lookup"><span data-stu-id="32c71-270">**Threading**</span></span>

- <span data-ttu-id="32c71-271">.NET CLR bloqueos y subprocesos\\número de subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="32c71-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="32c71-272">.NET CLR bloqueos y subprocesos\\número de subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="32c71-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="32c71-273">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="32c71-273">Other Resources</span></span>

<span data-ttu-id="32c71-274">Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="32c71-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="32c71-275">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="32c71-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="32c71-276">Uso de los subprocesos de ASP.NET en IIS 7.5, IIS 7.0 e IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="32c71-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="32c71-277">&lt;applicationPool&gt; elemento (configuración Web)</span><span class="sxs-lookup"><span data-stu-id="32c71-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
