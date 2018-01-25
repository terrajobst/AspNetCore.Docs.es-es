---
uid: signalr/overview/older-versions/signalr-performance
title: Rendimiento de SignalR (SignalR 1.x) | Documentos de Microsoft
author: pfletcher
description: Rendimiento de SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a>Rendimiento de SignalR (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.


Para ver una presentación reciente sobre el rendimiento de SignalR y ajuste de escala, vea [ajuste de escala en la Web en tiempo real mediante ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Este tema contiene las siguientes secciones:

- [Consideraciones de diseño](#design)
- [Optimización del servidor de SignalR para el rendimiento](#tuning)
- [Solucionar problemas de rendimiento](#troubleshooting)
- [Uso de contadores de rendimiento de SignalR](#perfcounters)
- [Uso de otros contadores de rendimiento](#othercounters)
- [Otros recursos](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Consideraciones de diseño

Esta sección describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que no se se impide el rendimiento mediante la generación de tráfico de red innecesario.

### <a name="throttling-message-frequency"></a>Límite de frecuencia de mensajes

Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no es necesario enviar más de unos pocos mensajes por segundo. Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que las colas y los envía no mensajes que supere con frecuencia una tarifa fija (es decir, hasta un número determinado de mensajes se enviará cada segundo, si hay mensajes en ese momento en terval para enviarse). Para una aplicación de ejemplo que limita los mensajes a una velocidad de cierta (desde el cliente y servidor), consulte [alta frecuencia en tiempo real con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Reducir el tamaño del mensaje

Puede reducir el tamaño de un mensaje de SignalR al reducir el tamaño de los objetos serializados. En el código de servidor, si va a enviar un objeto que contiene propiedades que no es necesario para que se transmitan, impedir que las propiedades que se va a serializar mediante el uso del `JsonIgnore` atributo. Los nombres de propiedades también se almacenan en el mensaje. se puede abreviar los nombres de propiedades con el `JsonProperty` atributo. El ejemplo de código siguiente muestra cómo excluir una propiedad de la que se envían al cliente y cómo acortar los nombres de propiedad:

**Código de servidor de .NET que muestra el atributo JsonIgnore para excluir los datos se envíen al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Para conservar la legibilidad / mantenimiento en el código de cliente, los nombres de propiedad abreviada pueden ser nombres reasignados a humanos sencillo cuando se recibe el mensaje. El ejemplo de código siguiente muestra una posible manera de volver a asignar nombres abreviados a más largos, mediante la definición de un contrato de mensaje (asignación) y el uso de la `reMap` función que se aplica el contrato a la clase optimizada de mensajes:

**Código de JavaScript del lado cliente que reasigna acorta los nombres de propiedad a los nombres de lenguaje natural**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Los nombres se puede abreviar en los mensajes desde el cliente al servidor, con el mismo método.

Reduce la superficie de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del mensaje de objeto también puede mejorar el rendimiento. Por ejemplo, si el intervalo completo de un `int` no es necesaria, un `short` o `byte` pueden utilizarse en su lugar.

Puesto que los mensajes se almacenan en el bus de mensajes en memoria del servidor, lo que reduce el tamaño de mensajes también puede solucionar problemas de memoria de servidor.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Optimización del servidor de SignalR para el rendimiento

Las siguientes opciones de configuración se pueden utilizar para optimizar el servidor para mejorar el rendimiento en una aplicación de SignalR. Para obtener información general sobre cómo mejorar el rendimiento de una aplicación ASP.NET, vea [mejorar el rendimiento de ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Opciones de configuración de SignalR**

- **DefaultMessageBufferSize**: de forma predeterminada, SignalR conserva 1000 mensajes en memoria por centro de cada conexión. Si se utilizan mensajes de gran tamaño, esto puede crear problemas de memoria que se pueden mitigar si reduce este valor. Esta configuración se puede establecer el `Application_Start` controlador de eventos en una aplicación ASP.NET, o en la `Configuration` método de una clase de inicio OWIN en una aplicación hospeda a sí mismo. El ejemplo siguiente muestra cómo reducir este valor con el fin de reducir la superficie de memoria de la aplicación con el fin de reducir la cantidad de memoria de servidor usada:

    **Código de servidor de .NET en Global.asax para reducir el tamaño de búfer de mensaje predeterminado**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Opciones de configuración de IIS**

- **Máximo de solicitudes simultáneas por aplicación**: aumentar el número de IIS simultáneas solicitudes aumentará disponible para atender las solicitudes de recursos de servidor. El valor predeterminado es 5000; Para aumentar este valor, ejecute los comandos siguientes en un símbolo del sistema con privilegios elevados:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Opciones de configuración de ASP.NET**

Esta sección incluye los valores de configuración que se pueden establecer en el `aspnet.config` archivo. Este archivo se encuentra en una de estas dos ubicaciones, dependiendo de la plataforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Configuración de ASP.NET que puede mejorar el rendimiento de SignalR incluye lo siguiente:

- **Número máximo de solicitudes simultáneo por CPU**: aumento de esta configuración puede mitigar los cuellos de botella de rendimiento. Para aumentar esta opción, agregue la siguiente opción de configuración para el `aspnet.config` archivo:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Límite de la cola de peticiones**: cuando se supera el número total de conexiones la `maxConcurrentRequestsPerCPU` configuración, ASP.NET comenzará la limitación de solicitudes mediante una cola. Para aumentar el tamaño de la cola, puede aumentar la `requestQueueLimit` configuración. Para ello, agregue la siguiente opción de configuración para el `processModel` nodo `config/machine.config` (en lugar de `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Solucionar problemas de rendimiento

Esta sección describe las formas de buscar los cuellos de botella de rendimiento en la aplicación.

### <a name="verifying-that-websocket-is-being-used"></a>Comprobar que se está usando WebSocket

Aunque SignalR puede usar una serie de transportes para la comunicación entre cliente y servidor, WebSocket ofrece una ventaja de rendimiento importante y debe usarse si el cliente y el servidor lo admiten. Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports). Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollo de explorador y examine los registros para ver qué transporte se utiliza para la conexión. Para obtener información sobre el uso de las herramientas de desarrollo de explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Uso de contadores de rendimiento de SignalR

Esta sección describe cómo habilitar y utilizar contadores de rendimiento de SignalR, se encuentra en la `Microsoft.AspNet.SignalR.Utils` paquete.

### <a name="installing-signalrexe"></a>Instalar signalr.exe

Contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada SignalR.exe. Para instalar esta utilidad, siga estos pasos:

1. En la aplicación de Visual Studio, seleccione **herramientas**, **Administrador de paquetes de biblioteca**, **administrar paquetes de NuGet para la solución...**
2. Busque **signalr.utils**y seleccione instalar.

    ![](signalr-performance/_static/image1.png)
3. Acepte el contrato de licencia para instalar el paquete.
4. SignalR.exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalando los contadores de rendimiento con SignalR.exe

Para instalar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el parámetro siguiente:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contadores de rendimiento de SignalR

El paquete de utilidades instala los siguientes contadores de rendimiento. Los contadores de "Total" medir el número de eventos desde el último grupo de aplicaciones o el servidor se reinicie.

**Métricas de conexión**

Las siguientes métricas de medir los eventos de duración de la conexión que se producen. Para obtener más información, consulte [descripción y control de eventos de duración de conexión](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Conexiones conectadas**
- **Conexiones a conectar**
- **Las conexiones Disonnected**
- **Conexiones actuales**

**Medidas de mensaje**

Las siguientes métricas de medir el tráfico de mensajes generado por SignalR.

- **Total de mensajes de conexión**
- **Total de mensajes de conexión enviados**
- **Conexión de mensajes recibidos/seg.**
- **Conexión de mensajes enviados/seg.**

**Métricas de bus de mensaje**

Las siguientes métricas de medir el tráfico a través del bus de mensajes de SignalR interno, la cola en el que se colocan todos los mensajes de SignalR entrantes y salientes. Un mensaje es **publicada** cuando se envían o difusión. A **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto es igual al número de clientes más el propio servidor. Un **trabajo asignado** es un componente que envía datos a las conexiones activas; una **trabajo ocupado** es aquella que está enviando activamente un mensaje.

- **Recibido Total de mensajes de Bus de mensajes**
- **Bus de mensajes mensajes recibidos/seg.**
- **Bus de mensajes de mensajes publican Total**
- **Bus de mensajes de mensajes publicados/seg.**
- **Actual de suscriptores de Bus de mensaje**
- **Total de suscriptores de Bus de mensajes**
- **Los suscriptores de Bus de mensajes/seg.**
- **Asignada los trabajadores de Bus de mensajes**
- **Trabajadores ocupados del Bus de mensaje**
- **Actual de temas del Bus de mensaje**

**Métricas de error**

Las siguientes métricas de medir los errores generados por el tráfico de mensajes de SignalR. **Resolución de concentrador** errores se producen cuando un concentrador o un método de concentrador no se puede resolver. **Invocación de concentrador** errores son las excepciones producidas al invocar un método de concentrador. **Transporte** los errores son errores de conexión que se produce durante una solicitud o respuesta HTTP.

- **Errores: Total de todos los**
- **Errores: All/seg.**
- **Errores: Total de resolución de concentrador**
- **Errores: Resolución de concentrador / seg.**
- **Errores: Total de invocación de concentrador**
- **Errores: Invocación de concentrador / seg.**
- **Errores: Total de transporte**
- **Errores: Transporte/seg.**

**Métricas de ampliación**

Las siguientes métricas de medir el tráfico y los errores generados por el proveedor de ampliación. A **flujo** en este contexto es una unidad de escala utilizada por el proveedor de ampliación; se trata de una tabla si se utiliza SQL Server, un tema si se usa el Bus de servicio y una suscripción si se usa Redis. De forma predeterminada, se usa sólo una secuencia, pero esto puede aumentar a través de la configuración de SQL Server y Service Bus. A **Buffering** flujo es aquel que ha entrado en un estado de error; cuando la secuencia está en estado de error, todos los mensajes enviados al backplane se producirá un error hasta que ya no es un error en la secuencia. El **longitud de cola de envío** es el número de mensajes que se han registrado pero aún no se ha enviado.

- **Bus de mensajes de ampliación mensajes recibidos/seg.**
- **Total de secuencias de ampliación**
- **Ampliación transmite por secuencias abierto**
- **Almacenamiento en búfer de secuencias de ampliación**
- **Total de errores de ampliación**
- **Errores de ampliación/seg.**
- **Longitud de la cola de envío de ampliación**

Para obtener más información sobre lo que va a medir estos contadores, vea [SignalR Scaleout con Bus de servicio de Azure](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Uso de otros contadores de rendimiento

Los siguientes contadores de rendimiento también pueden resultarle útiles para supervisar el rendimiento de la aplicación.

**Memoria**

- Memoria de .NET CLR # bytes en todos los montones (por w3wp)

**ASP.NET**

- ASP. NET\Solicitudes actuales
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Tiempo de procesador Information\Processor

**TCP/IP**

- TCPv6/conexiones establecidas
- TCPv4/conexiones establecidas

**Servicio Web**

- Web\conexiones
- Conexiones de Service\Maximum Web

**Subprocesamiento**

- LocksAndThreads de .NET CLR\# de subprocesos lógicos actuales
- Subprocesos de .NET CLR LocksAnd\# de subprocesos físicos actuales

<a id="otherresources"></a>

## <a name="other-resources"></a>Otros recursos

Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:

- [Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Uso de los subprocesos de ASP.NET en IIS 7.5, IIS 7.0 e IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; elemento (configuración Web)](https://msdn.microsoft.com/library/dd560842.aspx)
