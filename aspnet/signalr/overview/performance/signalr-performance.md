---
uid: signalr/overview/performance/signalr-performance
title: Rendimiento de SignalR | Microsoft Docs
author: pfletcher
description: Rendimiento de SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 9d032f84bbbc3ddb2e102cb37fd4d52ae5ef75c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388758"
---
<a name="signalr-performance"></a><span data-ttu-id="ac5f3-103">Rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-103">SignalR Performance</span></span>
====================
<span data-ttu-id="ac5f3-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ac5f3-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="ac5f3-105">Este tema describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ac5f3-106">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="ac5f3-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="ac5f3-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ac5f3-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="ac5f3-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ac5f3-108">.NET 4.5</span></span>
> - <span data-ttu-id="ac5f3-109">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ac5f3-110">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="ac5f3-111">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ac5f3-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="ac5f3-112">Questions and comments</span></span>
> 
> <span data-ttu-id="ac5f3-113">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ac5f3-114">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="ac5f3-115">Para ver una presentación reciente sobre el rendimiento de SignalR y escalado, consulte [escalado Web en tiempo real con SignalR de ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="ac5f3-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="ac5f3-117">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="ac5f3-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="ac5f3-118">Optimización de rendimiento de su servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="ac5f3-119">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="ac5f3-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="ac5f3-120">Mediante los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="ac5f3-121">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="ac5f3-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="ac5f3-122">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="ac5f3-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="ac5f3-123">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="ac5f3-123">Design considerations</span></span>

<span data-ttu-id="ac5f3-124">Esta sección describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que no se se impide el rendimiento mediante la generación de tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="ac5f3-125">Limitación de frecuencia de mensajes</span><span class="sxs-lookup"><span data-stu-id="ac5f3-125">Throttling message frequency</span></span>

<span data-ttu-id="ac5f3-126">Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no es necesario enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="ac5f3-127">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que las colas y envía no mensajes de más que con frecuencia de una tarifa fija (es decir, hasta un cierto número de mensajes se enviarán cada segundo, si hay mensajes en ese momento en tervalo para enviarse).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="ac5f3-128">Para una aplicación de ejemplo que limita los mensajes a la tasa de interés (de cliente y servidor), consulte [alta frecuencia en tiempo real con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="ac5f3-129">Disminuye el tamaño de mensaje</span><span class="sxs-lookup"><span data-stu-id="ac5f3-129">Reducing message size</span></span>

<span data-ttu-id="ac5f3-130">Puede reducir el tamaño de un mensaje de SignalR al reducir el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="ac5f3-131">En el código de servidor, si va a enviar un objeto que contiene propiedades que no es necesario que se transmitan, impedir que esas propiedades que se va a serializar utilizando la `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="ac5f3-132">Los nombres de propiedades también se almacenan en el mensaje. los nombres de propiedades pueden reducirse mediante el `JsonProperty` atributo.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="ac5f3-133">Ejemplo de código siguiente muestra cómo excluir una propiedad que se envíen al cliente y acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="ac5f3-134">**Código de servidor de .NET que muestra el atributo JsonIgnore para excluir los datos que se envíen al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="ac5f3-135">Con el fin de conservar la legibilidad y mantenimiento en el código de cliente, los nombres de propiedad abreviada pueden ser reasignado a fácil de usar nombres una vez recibido el mensaje.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="ac5f3-136">Ejemplo de código siguiente muestra una posible manera de volver a asignar nombres abreviados a más largos, definiendo un contrato de mensaje (asignación) y el uso de la `reMap` función que se aplica el contrato a la clase de mensajes optimizada:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="ac5f3-137">**Código de JavaScript del lado cliente que reasigna acorta los nombres de propiedad a los nombres de lenguaje natural**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="ac5f3-138">Los nombres se pueden abreviar en los mensajes desde el cliente al servidor, con el mismo método.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="ac5f3-139">Lo que reduce la superficie de memoria (es decir, la cantidad de memoria usada para el mensaje) del mensaje de objeto también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="ac5f3-140">Por ejemplo, si la gama completa de un `int` no es necesaria una `short` o `byte` se puede usar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="ac5f3-141">Puesto que los mensajes se almacenan en el bus de mensajes en memoria del servidor, lo que reduce el tamaño de mensajes también puede solucionar problemas de memoria de servidor.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="ac5f3-142">Optimización de rendimiento de su servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="ac5f3-143">Las siguientes opciones de configuración pueden usarse para optimizar el servidor para mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="ac5f3-144">Para obtener información general sobre cómo mejorar el rendimiento en una aplicación ASP.NET, vea [mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="ac5f3-145">**Opciones de configuración de SignalR**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="ac5f3-146">**DefaultMessageBufferSize**: de forma predeterminada, conserva los 1000 mensajes en la memoria por cada conexión de centro de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="ac5f3-147">Si se utilizan mensajes de gran tamaño, esto puede crear problemas de memoria que se pueden mitigar si reduce este valor.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="ac5f3-148">Esta configuración se puede establecer el `Application_Start` controlador de eventos en una aplicación ASP.NET o en el `Configuration` método de una clase de inicio OWIN en una aplicación autohospedada.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="ac5f3-149">El ejemplo siguiente muestra cómo reducir este valor con el fin de reducir el consumo de memoria de la aplicación con el fin de reducir la cantidad de memoria de servidor usada:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="ac5f3-150">**Código de servidor de .NET para reducir el tamaño de búfer de mensajes de forma predeterminada en Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="ac5f3-151">**Opciones de configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="ac5f3-152">**Máximo de solicitudes simultáneas por cada aplicación**: aumento del número de IIS simultáneas solicitudes aumentará disponibles para atender las solicitudes de recursos de servidor.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="ac5f3-153">El valor predeterminado es 5000; Para aumentar este valor, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="ac5f3-154">**ApplicationPool QueueLength**: este es el número máximo de solicitudes que Http.sys pone en cola para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="ac5f3-155">Cuando la cola está llena, nuevas solicitudes reciben una respuesta 503 "Servicio no disponible".</span><span class="sxs-lookup"><span data-stu-id="ac5f3-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="ac5f3-156">El valor predeterminado es 1000.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-156">The default value is 1000.</span></span>

    <span data-ttu-id="ac5f3-157">Acortar la longitud de cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación se conservará los recursos de memoria.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="ac5f3-158">Para obtener más información, consulte [configurar grupos de aplicaciones, administración y optimización](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="ac5f3-159">**Opciones de configuración de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="ac5f3-160">Esta sección incluye opciones de configuración que se pueden establecer en el `aspnet.config` archivo.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="ac5f3-161">Este archivo se encuentra en una de estas dos ubicaciones, dependiendo de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="ac5f3-162">Configuración de ASP.NET que puede mejorar el rendimiento de SignalR incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="ac5f3-163">**Número máximo de solicitudes simultáneo por CPU**: aumento de esta configuración podría aliviar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="ac5f3-164">Para aumentar este valor, agregue la siguiente opción de configuración para el `aspnet.config` archivo:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="ac5f3-165">**Límite de cola de solicitudes**: cuando se supera el número total de conexiones del `maxConcurrentRequestsPerCPU` establecer, ASP.NET comenzará con una cola de solicitudes de limitación.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="ac5f3-166">Para aumentar el tamaño de la cola, puede aumentar el `requestQueueLimit` configuración.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="ac5f3-167">Para ello, agregue la siguiente opción de configuración para el `processModel` nodo `config/machine.config` (en lugar de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="ac5f3-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="ac5f3-168">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="ac5f3-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="ac5f3-169">Esta sección describen formas de buscar los cuellos de botella en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="ac5f3-170">Comprobar que se utiliza WebSocket</span><span class="sxs-lookup"><span data-stu-id="ac5f3-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="ac5f3-171">Aunque SignalR puede usar una variedad de transportes para la comunicación entre cliente y servidor, WebSocket ofrece una importante ventaja de rendimiento y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="ac5f3-172">Para determinar si el cliente y servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="ac5f3-173">Para determinar qué transporte se utiliza en la aplicación, puede usar las herramientas para desarrolladores del explorador y examinar los registros para ver qué transporte se utiliza para la conexión.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="ac5f3-174">Para obtener información sobre el uso de las herramientas de desarrollo del explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="ac5f3-175">Mediante los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-175">Using SignalR performance counters</span></span>

<span data-ttu-id="ac5f3-176">Esta sección describe cómo habilitar y utilizar contadores de rendimiento de SignalR, se encuentra en el `Microsoft.AspNet.SignalR.Utils` paquete.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="ac5f3-177">Instalar signalr.exe</span><span class="sxs-lookup"><span data-stu-id="ac5f3-177">Installing signalr.exe</span></span>

<span data-ttu-id="ac5f3-178">Contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="ac5f3-179">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="ac5f3-180">En la aplicación de Visual Studio, seleccione **herramientas**, **Administrador de paquetes de biblioteca**, **administrar paquetes de NuGet para la solución...**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="ac5f3-181">Busque **signalr.utils**y seleccione instalar.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="ac5f3-182">Acepte el contrato de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="ac5f3-183">SignalR.exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="ac5f3-184">Instalando contadores de rendimiento con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="ac5f3-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="ac5f3-185">Para instalar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="ac5f3-186">Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="ac5f3-187">Contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="ac5f3-187">SignalR Performance counters</span></span>

<span data-ttu-id="ac5f3-188">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="ac5f3-189">Los contadores de "Total" medir el número de eventos desde el último grupo de aplicaciones o el servidor que se reinició.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="ac5f3-190">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-190">**Connection metrics**</span></span>

<span data-ttu-id="ac5f3-191">Las siguientes métricas miden los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="ac5f3-192">Para obtener más información, consulte [comprensión y control de eventos de duración de conexión](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="ac5f3-193">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-193">**Connections Connected**</span></span>
- <span data-ttu-id="ac5f3-194">**Conexiones que se puede volver a conectar**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="ac5f3-195">**Conexiones desconectado**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="ac5f3-196">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-196">**Connections Current**</span></span>

<span data-ttu-id="ac5f3-197">**Medidas de mensaje**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-197">**Message metrics**</span></span>

<span data-ttu-id="ac5f3-198">Las siguientes métricas de medir el tráfico de mensajes generado por SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="ac5f3-199">**Total de mensajes de conexión**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="ac5f3-200">**Total de mensajes de conexión enviados**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="ac5f3-201">**Conexión de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="ac5f3-202">**Conexión de los mensajes enviados/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="ac5f3-203">**Métricas de bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-203">**Message bus metrics**</span></span>

<span data-ttu-id="ac5f3-204">Las siguientes métricas de medir el tráfico a través del bus de mensajes de SignalR interno, la cola en el que se colocan todos los mensajes de SignalR entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="ac5f3-205">Es un mensaje **publicada** cuando se envían o difundir.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="ac5f3-206">Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto debe ser igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="ac5f3-207">Un **trabajo asignado** es un componente que envía datos a las conexiones activas; una **trabajo ocupado** es aquella que está enviando activamente un mensaje.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="ac5f3-208">**Recibido Total de mensajes de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="ac5f3-209">**Bus de mensajes de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="ac5f3-210">**Bus de mensajes de mensajes publican Total**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="ac5f3-211">**Bus de mensajes de mensajes publicados/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="ac5f3-212">**Actual de los suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="ac5f3-213">**Total de suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="ac5f3-214">**Los suscriptores de Bus de mensajes/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="ac5f3-215">**Asignado a los trabajadores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="ac5f3-216">**Trabajadores ocupados de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="ac5f3-217">**Actual de temas del Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="ac5f3-218">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-218">**Error metrics**</span></span>

<span data-ttu-id="ac5f3-219">Las siguientes métricas de medir los errores generados por el tráfico de mensajes de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="ac5f3-220">**Resolución de concentrador** errores se producen cuando un concentrador o un método de concentrador no se puede resolver.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="ac5f3-221">**Invocación de concentrador** errores son las excepciones iniciadas al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="ac5f3-222">**Transporte** errores son errores de conexión que se produce durante una solicitud o respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="ac5f3-223">**Errores: Total de todos los**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-223">**Errors: All Total**</span></span>
- <span data-ttu-id="ac5f3-224">**Errores: All/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="ac5f3-225">**Errores: Total de resolución de concentrador**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="ac5f3-226">**Errores: Resolución de concentrador / seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="ac5f3-227">**Errores: Total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="ac5f3-228">**Errores: Invocación de concentrador/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="ac5f3-229">**Errores: Total de transporte**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="ac5f3-230">**Errores: Transporte/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="ac5f3-231">**Métricas de escalado horizontal**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-231">**Scaleout metrics**</span></span>

<span data-ttu-id="ac5f3-232">Las siguientes métricas de medir el tráfico y los errores generados por el proveedor de ampliación.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="ac5f3-233">Un **Stream** en este contexto es una unidad de escala utilizada por el proveedor de escalabilidad horizontal; se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="ac5f3-234">Garantiza que cada flujo ordenado operaciones de lectura y escritura; una única secuencia es un posible cuello de botella de escalabilidad, por lo que se puede aumentar el número de secuencias para ayudar a reducir un cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="ac5f3-235">Si se utilizan varias secuencias, SignalR distribuirá automáticamente los mensajes (partición) entre estos flujos de forma que garantiza que los mensajes enviados desde las conexiones están en orden.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="ac5f3-236">El [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) opción controla la longitud de la cola de envío de ampliación y mantenida por SignalR.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="ac5f3-237">Si se establece en un valor mayor que 0 colocará todos los mensajes en una cola de envío para enviarse una vez al backplane de mensajería configurado.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="ac5f3-238">Si el tamaño de la cola supere la duración configurada, las llamadas subsiguientes a enviar will dará error inmediatamente con un [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes de la cola es menor que el valor nuevo.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="ac5f3-239">Puesta en cola está deshabilitada de forma predeterminada porque los planos posteriores implementadas generalmente tienen su propio control de flujo o de puesta en cola en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="ac5f3-240">En el caso de SQL Server, la agrupación de conexiones eficazmente limita el número de envíos sucediendo en todo momento.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="ac5f3-241">De forma predeterminada, se usa sólo una secuencia para SQL Server y Redis, cinco flujos sirven para Service Bus y está deshabilitada la puesta en cola, pero esta configuración puede cambiarse a través de la configuración de SQL Server y Service Bus:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="ac5f3-242">**Código de servidor .NET para configurar la longitud de cola y recuento de tabla para el plano posterior de SQL Server**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="ac5f3-243">**Código de servidor .NET para configurar la longitud de cola y recuento de tema de Service Bus backplane**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="ac5f3-244">Un **Buffering** secuencia es aquel que ha entrado en un estado de error; cuando la secuencia está en estado de error, todos los mensajes enviados al backplane se producirá un error inmediato hasta que la secuencia ya no se produzca un error.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="ac5f3-245">El **longitud de cola de envío** es el número de mensajes que se han registrado, pero no se han enviado.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="ac5f3-246">**Bus de mensajes de ampliación de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="ac5f3-247">**Total de secuencias de escalabilidad horizontal**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="ac5f3-248">**Ampliación y transmite abierto**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="ac5f3-249">**Almacenamiento en búfer de flujos de escalabilidad horizontal**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="ac5f3-250">**Total de errores de ampliación**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="ac5f3-251">**Escalabilidad horizontal de errores/seg.**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="ac5f3-252">**Longitud de cola de envío de ampliación**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="ac5f3-253">Para obtener más información sobre lo que va a medir estos contadores, vea [SignalR Scaleout con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="ac5f3-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="ac5f3-254">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="ac5f3-254">Using other performance counters</span></span>

<span data-ttu-id="ac5f3-255">Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ac5f3-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="ac5f3-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-256">**Memory**</span></span>

- <span data-ttu-id="ac5f3-257">Memoria de .NET CLR\\número de bytes de todos los montones (por w3wp)</span><span class="sxs-lookup"><span data-stu-id="ac5f3-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="ac5f3-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-258">**ASP.NET**</span></span>

- <span data-ttu-id="ac5f3-259">ASP. NET\Solicitudes actuales</span><span class="sxs-lookup"><span data-stu-id="ac5f3-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="ac5f3-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="ac5f3-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="ac5f3-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="ac5f3-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="ac5f3-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-262">**CPU**</span></span>

- <span data-ttu-id="ac5f3-263">Tiempo de procesador Information\Processor</span><span class="sxs-lookup"><span data-stu-id="ac5f3-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="ac5f3-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-264">**TCP/IP**</span></span>

- <span data-ttu-id="ac5f3-265">TCPv6/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="ac5f3-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="ac5f3-266">TCPv4/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="ac5f3-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="ac5f3-267">**Servicio Web**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-267">**Web Service**</span></span>

- <span data-ttu-id="ac5f3-268">Conexiones de servicio web\conexiones Web</span><span class="sxs-lookup"><span data-stu-id="ac5f3-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="ac5f3-269">Conexiones Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="ac5f3-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="ac5f3-270">**Subprocesamiento**</span><span class="sxs-lookup"><span data-stu-id="ac5f3-270">**Threading**</span></span>

- <span data-ttu-id="ac5f3-271">.NET CLR de bloqueos y subprocesos\\número de subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="ac5f3-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="ac5f3-272">.NET CLR de bloqueos y subprocesos\\número de subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="ac5f3-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="ac5f3-273">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="ac5f3-273">Other Resources</span></span>

<span data-ttu-id="ac5f3-274">Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ac5f3-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="ac5f3-275">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="ac5f3-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="ac5f3-276">Uso de subprocesos de ASP.NET en IIS 6.0, IIS 7.0 y IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="ac5f3-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="ac5f3-277">&lt;applicationPool&gt; elemento (configuración Web)</span><span class="sxs-lookup"><span data-stu-id="ac5f3-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
