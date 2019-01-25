---
uid: signalr/overview/older-versions/signalr-performance
title: Rendimiento de SignalR (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Rendimiento de SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4158cb055088f3da752020e577007ffe80856b60
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837733"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="6be10-103">Rendimiento de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="6be10-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="6be10-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="6be10-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="6be10-105">Este tema describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="6be10-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="6be10-106">Para ver una presentación reciente sobre el rendimiento de SignalR y escalado, consulte [escalado Web en tiempo real con SignalR de ASP.NET](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="6be10-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="6be10-107">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="6be10-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6be10-108">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="6be10-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="6be10-109">Optimización de rendimiento de su servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="6be10-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="6be10-110">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="6be10-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="6be10-111">Mediante los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="6be10-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="6be10-112">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="6be10-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="6be10-113">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="6be10-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="6be10-114">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="6be10-114">Design considerations</span></span>

<span data-ttu-id="6be10-115">Esta sección describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que no se se impide el rendimiento mediante la generación de tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="6be10-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="6be10-116">Limitación de frecuencia de mensajes</span><span class="sxs-lookup"><span data-stu-id="6be10-116">Throttling message frequency</span></span>

<span data-ttu-id="6be10-117">Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no es necesario enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="6be10-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="6be10-118">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que las colas y envía no mensajes de más que con frecuencia de una tarifa fija (es decir, hasta un cierto número de mensajes se enviarán cada segundo, si hay mensajes en ese momento en tervalo para enviarse).</span><span class="sxs-lookup"><span data-stu-id="6be10-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="6be10-119">Para una aplicación de ejemplo que limita los mensajes a la tasa de interés (de cliente y servidor), consulte [alta frecuencia en tiempo real con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="6be10-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="6be10-120">Disminuye el tamaño de mensaje</span><span class="sxs-lookup"><span data-stu-id="6be10-120">Reducing message size</span></span>

<span data-ttu-id="6be10-121">Puede reducir el tamaño de un mensaje de SignalR al reducir el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="6be10-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="6be10-122">En el código de servidor, si va a enviar un objeto que contiene propiedades que no es necesario que se transmitan, impedir que esas propiedades que se va a serializar utilizando la `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="6be10-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="6be10-123">Los nombres de propiedades también se almacenan en el mensaje. los nombres de propiedades pueden reducirse mediante el `JsonProperty` atributo.</span><span class="sxs-lookup"><span data-stu-id="6be10-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="6be10-124">Ejemplo de código siguiente muestra cómo excluir una propiedad que se envíen al cliente y acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="6be10-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="6be10-125">**Código de servidor de .NET que muestra el atributo JsonIgnore para excluir los datos que se envíen al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="6be10-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="6be10-126">Con el fin de conservar la legibilidad y mantenimiento en el código de cliente, los nombres de propiedad abreviada pueden ser reasignado a fácil de usar nombres una vez recibido el mensaje.</span><span class="sxs-lookup"><span data-stu-id="6be10-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="6be10-127">Ejemplo de código siguiente muestra una posible manera de volver a asignar nombres abreviados a más largos, definiendo un contrato de mensaje (asignación) y el uso de la `reMap` función que se aplica el contrato a la clase de mensajes optimizada:</span><span class="sxs-lookup"><span data-stu-id="6be10-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="6be10-128">**Código de JavaScript del lado cliente que reasigna acorta los nombres de propiedad a los nombres de lenguaje natural**</span><span class="sxs-lookup"><span data-stu-id="6be10-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="6be10-129">Los nombres se pueden abreviar en los mensajes desde el cliente al servidor, con el mismo método.</span><span class="sxs-lookup"><span data-stu-id="6be10-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="6be10-130">Lo que reduce la superficie de memoria (es decir, la cantidad de memoria usada para el mensaje) del mensaje de objeto también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="6be10-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="6be10-131">Por ejemplo, si la gama completa de un `int` no es necesaria una `short` o `byte` se puede usar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="6be10-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="6be10-132">Puesto que los mensajes se almacenan en el bus de mensajes en memoria del servidor, lo que reduce el tamaño de mensajes también puede solucionar problemas de memoria de servidor.</span><span class="sxs-lookup"><span data-stu-id="6be10-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="6be10-133">Optimización de rendimiento de su servidor de SignalR</span><span class="sxs-lookup"><span data-stu-id="6be10-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="6be10-134">Las siguientes opciones de configuración pueden usarse para optimizar el servidor para mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="6be10-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="6be10-135">Para obtener información general sobre cómo mejorar el rendimiento en una aplicación ASP.NET, vea [mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="6be10-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="6be10-136">**Opciones de configuración de SignalR**</span><span class="sxs-lookup"><span data-stu-id="6be10-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="6be10-137">**DefaultMessageBufferSize**: De forma predeterminada, SignalR conserva los mensajes de 1000 en memoria por centro por conexión.</span><span class="sxs-lookup"><span data-stu-id="6be10-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="6be10-138">Si se utilizan mensajes de gran tamaño, esto puede crear problemas de memoria que se pueden mitigar si reduce este valor.</span><span class="sxs-lookup"><span data-stu-id="6be10-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="6be10-139">Esta configuración se puede establecer el `Application_Start` controlador de eventos en una aplicación ASP.NET o en el `Configuration` método de una clase de inicio OWIN en una aplicación autohospedada.</span><span class="sxs-lookup"><span data-stu-id="6be10-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="6be10-140">El ejemplo siguiente muestra cómo reducir este valor con el fin de reducir el consumo de memoria de la aplicación con el fin de reducir la cantidad de memoria de servidor usada:</span><span class="sxs-lookup"><span data-stu-id="6be10-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="6be10-141">**Código de servidor de .NET para la reducción del tamaño de búfer de mensaje predeterminado en Global.asax**</span><span class="sxs-lookup"><span data-stu-id="6be10-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="6be10-142">**Opciones de configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="6be10-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="6be10-143">**Máximo de solicitudes simultáneas por cada aplicación**: Aumentar el número de IIS simultáneas solicitudes aumentará disponibles para atender las solicitudes de recursos de servidor.</span><span class="sxs-lookup"><span data-stu-id="6be10-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="6be10-144">El valor predeterminado es 5000; Para aumentar este valor, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="6be10-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="6be10-145">**Opciones de configuración de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="6be10-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="6be10-146">Esta sección incluye opciones de configuración que se pueden establecer en el `aspnet.config` archivo.</span><span class="sxs-lookup"><span data-stu-id="6be10-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="6be10-147">Este archivo se encuentra en una de estas dos ubicaciones, dependiendo de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="6be10-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="6be10-148">Configuración de ASP.NET que puede mejorar el rendimiento de SignalR incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6be10-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="6be10-149">**Número máximo de solicitudes simultáneo por CPU**: Aumento de esta configuración podría aliviar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="6be10-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="6be10-150">Para aumentar este valor, agregue la siguiente opción de configuración para el `aspnet.config` archivo:</span><span class="sxs-lookup"><span data-stu-id="6be10-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="6be10-151">**Límite de cola de solicitudes**: Cuando se supera el número total de conexiones del `maxConcurrentRequestsPerCPU` establecer, ASP.NET comenzará con una cola de solicitudes de limitación.</span><span class="sxs-lookup"><span data-stu-id="6be10-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="6be10-152">Para aumentar el tamaño de la cola, puede aumentar el `requestQueueLimit` configuración.</span><span class="sxs-lookup"><span data-stu-id="6be10-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="6be10-153">Para ello, agregue la siguiente opción de configuración para el `processModel` nodo `config/machine.config` (en lugar de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="6be10-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="6be10-154">Solucionar problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="6be10-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="6be10-155">Esta sección describen formas de buscar los cuellos de botella en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6be10-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="6be10-156">Comprobar que se utiliza WebSocket</span><span class="sxs-lookup"><span data-stu-id="6be10-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="6be10-157">Aunque SignalR puede usar una variedad de transportes para la comunicación entre cliente y servidor, WebSocket ofrece una importante ventaja de rendimiento y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="6be10-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="6be10-158">Para determinar si el cliente y servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6be10-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="6be10-159">Para determinar qué transporte se utiliza en la aplicación, puede usar las herramientas para desarrolladores del explorador y examinar los registros para ver qué transporte se utiliza para la conexión.</span><span class="sxs-lookup"><span data-stu-id="6be10-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="6be10-160">Para obtener información sobre el uso de las herramientas de desarrollo del explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6be10-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="6be10-161">Mediante los contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="6be10-161">Using SignalR performance counters</span></span>

<span data-ttu-id="6be10-162">Esta sección describe cómo habilitar y utilizar contadores de rendimiento de SignalR, se encuentra en el `Microsoft.AspNet.SignalR.Utils` paquete.</span><span class="sxs-lookup"><span data-stu-id="6be10-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="6be10-163">Instalar signalr.exe</span><span class="sxs-lookup"><span data-stu-id="6be10-163">Installing signalr.exe</span></span>

<span data-ttu-id="6be10-164">En el servidor mediante una utilidad denominada SignalR.exe se pueden agregar contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="6be10-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="6be10-165">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="6be10-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="6be10-166">En Visual Studio, seleccione **herramientas** > **Administrador de paquetes de NuGet** > **administrar paquetes de NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="6be10-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="6be10-167">Busque **signalr.utils**y seleccione instalar.</span><span class="sxs-lookup"><span data-stu-id="6be10-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="6be10-168">Acepte el contrato de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="6be10-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="6be10-169">SignalR.exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="6be10-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="6be10-170">Instalando contadores de rendimiento con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="6be10-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="6be10-171">Para instalar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="6be10-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="6be10-172">Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:</span><span class="sxs-lookup"><span data-stu-id="6be10-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="6be10-173">Contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="6be10-173">SignalR Performance counters</span></span>

<span data-ttu-id="6be10-174">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="6be10-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="6be10-175">Los contadores de "Total" medir el número de eventos desde el último grupo de aplicaciones o el servidor que se reinició.</span><span class="sxs-lookup"><span data-stu-id="6be10-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="6be10-176">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="6be10-176">**Connection metrics**</span></span>

<span data-ttu-id="6be10-177">Las siguientes métricas miden los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="6be10-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="6be10-178">Para obtener más información, consulte [comprensión y control de eventos de duración de conexión](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6be10-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="6be10-179">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="6be10-179">**Connections Connected**</span></span>
- <span data-ttu-id="6be10-180">**Conexiones que se puede volver a conectar**</span><span class="sxs-lookup"><span data-stu-id="6be10-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="6be10-181">**Conexiones Disonnected**</span><span class="sxs-lookup"><span data-stu-id="6be10-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="6be10-182">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="6be10-182">**Connections Current**</span></span>

<span data-ttu-id="6be10-183">**Medidas de mensaje**</span><span class="sxs-lookup"><span data-stu-id="6be10-183">**Message metrics**</span></span>

<span data-ttu-id="6be10-184">Las siguientes métricas de medir el tráfico de mensajes generado por SignalR.</span><span class="sxs-lookup"><span data-stu-id="6be10-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="6be10-185">**Total de mensajes de conexión**</span><span class="sxs-lookup"><span data-stu-id="6be10-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="6be10-186">**Total de mensajes de conexión enviados**</span><span class="sxs-lookup"><span data-stu-id="6be10-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="6be10-187">**Conexión de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="6be10-188">**Conexión de los mensajes enviados/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="6be10-189">**Métricas de bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-189">**Message bus metrics**</span></span>

<span data-ttu-id="6be10-190">Las siguientes métricas de medir el tráfico a través del bus de mensajes de SignalR interno, la cola en el que se colocan todos los mensajes de SignalR entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="6be10-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="6be10-191">Es un mensaje **publicada** cuando se envían o difundir.</span><span class="sxs-lookup"><span data-stu-id="6be10-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="6be10-192">Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto debe ser igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="6be10-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="6be10-193">Un **trabajo asignado** es un componente que envía datos a las conexiones activas; una **trabajo ocupado** es aquella que está enviando activamente un mensaje.</span><span class="sxs-lookup"><span data-stu-id="6be10-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="6be10-194">**Recibido Total de mensajes de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="6be10-195">**Bus de mensajes de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6be10-196">**Bus de mensajes de mensajes publican Total**</span><span class="sxs-lookup"><span data-stu-id="6be10-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="6be10-197">**Bus de mensajes de mensajes publicados/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="6be10-198">**Actual de los suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="6be10-199">**Total de suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="6be10-200">**Los suscriptores de Bus de mensajes/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="6be10-201">**Asignado a los trabajadores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="6be10-202">**Trabajadores ocupados de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="6be10-203">**Actual de temas del Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="6be10-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="6be10-204">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="6be10-204">**Error metrics**</span></span>

<span data-ttu-id="6be10-205">Las siguientes métricas de medir los errores generados por el tráfico de mensajes de SignalR.</span><span class="sxs-lookup"><span data-stu-id="6be10-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="6be10-206">**Resolución de concentrador** errores se producen cuando un concentrador o un método de concentrador no se puede resolver.</span><span class="sxs-lookup"><span data-stu-id="6be10-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="6be10-207">**Invocación de concentrador** errores son las excepciones iniciadas al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6be10-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="6be10-208">**Transporte** errores son errores de conexión que se produce durante una solicitud o respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6be10-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="6be10-209">**Errores: Total de todos los**</span><span class="sxs-lookup"><span data-stu-id="6be10-209">**Errors: All Total**</span></span>
- <span data-ttu-id="6be10-210">**Errores: All/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="6be10-211">**Errores: Total de resolución de concentrador**</span><span class="sxs-lookup"><span data-stu-id="6be10-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="6be10-212">**Errores: Resolución de concentrador/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="6be10-213">**Errores: Total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="6be10-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="6be10-214">**Errores: Invocación de concentrador/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="6be10-215">**Errores: Total de transporte**</span><span class="sxs-lookup"><span data-stu-id="6be10-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="6be10-216">**Errores: Transport/Sec**</span><span class="sxs-lookup"><span data-stu-id="6be10-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="6be10-217">**Métricas de escalado horizontal**</span><span class="sxs-lookup"><span data-stu-id="6be10-217">**Scaleout metrics**</span></span>

<span data-ttu-id="6be10-218">Las siguientes métricas de medir el tráfico y los errores generados por el proveedor de ampliación.</span><span class="sxs-lookup"><span data-stu-id="6be10-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="6be10-219">Un **Stream** en este contexto es una unidad de escala utilizada por el proveedor de escalabilidad horizontal; se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="6be10-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="6be10-220">De forma predeterminada, se usa sólo una secuencia, pero se puede aumentar a través de la configuración de SQL Server y Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6be10-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="6be10-221">Un **Buffering** secuencia es aquel que ha entrado en un estado de error; cuando la secuencia está en estado de error, todos los mensajes enviados al backplane se producirá un error inmediato hasta que la secuencia ya no se produzca un error.</span><span class="sxs-lookup"><span data-stu-id="6be10-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="6be10-222">El **longitud de cola de envío** es el número de mensajes que se han registrado, pero no se han enviado.</span><span class="sxs-lookup"><span data-stu-id="6be10-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="6be10-223">**Bus de mensajes de ampliación de los mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="6be10-224">**Total de secuencias de escalabilidad horizontal**</span><span class="sxs-lookup"><span data-stu-id="6be10-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="6be10-225">**Ampliación y transmite abierto**</span><span class="sxs-lookup"><span data-stu-id="6be10-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="6be10-226">**Almacenamiento en búfer de flujos de escalabilidad horizontal**</span><span class="sxs-lookup"><span data-stu-id="6be10-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="6be10-227">**Total de errores de ampliación**</span><span class="sxs-lookup"><span data-stu-id="6be10-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="6be10-228">**Escalabilidad horizontal de errores/seg.**</span><span class="sxs-lookup"><span data-stu-id="6be10-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="6be10-229">**Longitud de cola de envío de ampliación**</span><span class="sxs-lookup"><span data-stu-id="6be10-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="6be10-230">Para obtener más información sobre lo que va a medir estos contadores, vea [SignalR Scaleout con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="6be10-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="6be10-231">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="6be10-231">Using other performance counters</span></span>

<span data-ttu-id="6be10-232">Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6be10-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="6be10-233">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="6be10-233">**Memory**</span></span>

- <span data-ttu-id="6be10-234">Memoria de .NET CLR # bytes en todos los montones (por w3wp)</span><span class="sxs-lookup"><span data-stu-id="6be10-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="6be10-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="6be10-235">**ASP.NET**</span></span>

- <span data-ttu-id="6be10-236">ASP. NET\Solicitudes actuales</span><span class="sxs-lookup"><span data-stu-id="6be10-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="6be10-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="6be10-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="6be10-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="6be10-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="6be10-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="6be10-239">**CPU**</span></span>

- <span data-ttu-id="6be10-240">Tiempo de procesador Information\Processor</span><span class="sxs-lookup"><span data-stu-id="6be10-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="6be10-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="6be10-241">**TCP/IP**</span></span>

- <span data-ttu-id="6be10-242">TCPv6/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="6be10-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="6be10-243">TCPv4/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="6be10-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="6be10-244">**Servicio Web**</span><span class="sxs-lookup"><span data-stu-id="6be10-244">**Web Service**</span></span>

- <span data-ttu-id="6be10-245">Conexiones de servicio web\conexiones Web</span><span class="sxs-lookup"><span data-stu-id="6be10-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="6be10-246">Conexiones Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="6be10-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="6be10-247">**Subprocesamiento**</span><span class="sxs-lookup"><span data-stu-id="6be10-247">**Threading**</span></span>

- <span data-ttu-id="6be10-248">LocksAndThreads de .NET CLR\# de subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="6be10-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="6be10-249">Subprocesos de .NET CLR LocksAnd\# de subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="6be10-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="6be10-250">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="6be10-250">Other Resources</span></span>

<span data-ttu-id="6be10-251">Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="6be10-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="6be10-252">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="6be10-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="6be10-253">Uso de subprocesos de ASP.NET en IIS 6.0, IIS 7.0 y IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="6be10-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="6be10-254">&lt;applicationPool&gt; elemento (configuración Web)</span><span class="sxs-lookup"><span data-stu-id="6be10-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
