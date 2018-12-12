---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Introducción a la escalabilidad horizontal en SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: fffa424ea4b62a54b9df48aaa409541ab5d1608f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287603"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Introducción a la escalabilidad horizontal en SignalR 1.x
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En general, hay dos formas de escalar una aplicación web: *escalar verticalmente* y *escalar horizontalmente*.

- Escalar verticalmente consiste en usar un servidor más grande (o una máquina virtual mayor) con más memoria RAM, CPU, etcetera.
- Escalar horizontalmente significa agregar más servidores para administrar la carga.

El problema con el escalado es que rápidamente alcanzar un límite en el tamaño de la máquina. Más allá de eso, deberá escalar horizontalmente. Sin embargo, al escalar horizontalmente, los clientes pueden obtener enrutar a diferentes servidores. Un cliente que está conectado a un servidor no recibirá los mensajes enviados desde otro servidor.

![](scaleout-in-signalr/_static/image1.png)

Una solución consiste en reenviar los mensajes entre servidores, mediante un componente denominado un *backplane*. Con un backplane habilitado, cada instancia de la aplicación envía mensajes al backplane y el backplane reenvía a las otras instancias de la aplicación. (En electrónica, un backplane es un grupo de conectores paralelos. Por analogía, un backplane SignalR conecta varios servidores.)

![](scaleout-in-signalr/_static/image2.png)

SignalR proporciona actualmente tres paneles posteriores:

- **Azure Service Bus**. Service Bus es una infraestructura de mensajería que permite a los componentes enviar mensajes en una forma escasamente acoplada.
- **Redis**. Redis es un almacén de pares clave-valor en memoria. Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.
- **SQL Server**. El plano posterior de SQL Server escribe mensajes en las tablas SQL. El backplane utiliza a Service Broker para la mensajería eficaz. Sin embargo, también funciona si no está habilitado Service Broker.

Si implementa la aplicación en Azure, considere el uso de la placa posterior de Azure Service Bus. Si va a implementar en su propia granja de servidores, considere la posibilidad de SQL Server o posteriores de Redis.

Los temas siguientes contienen tutoriales paso a paso para cada plano anterior:

- [Escalabilidad horizontal de SignalR con Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Escalabilidad horizontal de SignalR con Redis](scaleout-with-redis.md)
- [Escalabilidad horizontal de SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementación

En SignalR, todos los mensajes se envían a través de un bus de mensajes. Un bus de mensajes se implementa el [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaz, que proporciona una abstracción de publicación/suscripción. Funcionan los planos posteriores reemplazando el valor predeterminado **IMessageBus** con un bus diseñado por el backplane. Por ejemplo, el bus de mensajes de Redis es [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), y usa Redis [pub/sub](http://redis.io/topics/pubsub) mecanismo para enviar y recibir mensajes.

Cada instancia del servidor se conecta a la placa posterior a través del bus. Cuando se envía un mensaje, pasa a la placa posterior y el backplane lo envía a todos los servidores. Cuando un servidor recibe un mensaje del backplane, coloca el mensaje en su memoria caché local. El servidor, a continuación, envía mensajes a los clientes de su caché local.

Para cada conexión de cliente, se realiza un seguimiento progreso del cliente en la lectura de la secuencia de mensajes mediante un cursor. (Un cursor representa una posición en la secuencia de mensajes). Si un cliente se desconecta y, a continuación, se vuelve a conectar, solicita el bus de los mensajes que llegan después el valor del cursor del cliente. Lo mismo sucede cuando se utiliza una conexión [sondeo largo](../getting-started/introduction-to-signalr.md#transports). Una vez completada una solicitud de sondeo largo, el cliente abre una nueva conexión y pide a los mensajes recibidos después del cursor.

El mecanismo funciona el cursor incluso si un cliente se enruta a un servidor diferente en volver a conectar. El backplane es compatible con todos los servidores y no importa qué servidor se conecta un cliente.

## <a name="limitations"></a>Limitaciones

Usa un backplane, el rendimiento máximo del mensaje es menor que cuando los clientes comunicarse directamente con un solo nodo de servidor. Eso es porque el backplane reenvía todos los mensajes en todos los nodos, por lo que la placa posterior puede convertirse en un cuello de botella. Si esta limitación es un problema depende de la aplicación. Por ejemplo, estos son algunos escenarios típicos de SignalR:

- [Difusión de servidor](tutorial-server-broadcast-with-aspnet-signalr.md) (p. ej., tablero de cotizaciones): Planos posteriores funcionan bien para este escenario, porque el servidor controla la velocidad a la que se envían los mensajes.
- [Cliente a cliente](tutorial-getting-started-with-signalr.md) (p. ej., chat): En este escenario, el backplane podría ser un cuello de botella si el número de mensajes se escala con el número de clientes; es decir, si aumenta la tasa de mensajes unir más proporcionalmente como clientes.
- [En tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): No se recomienda un backplane para este escenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitar el seguimiento de escalabilidad horizontal de SignalR

Para habilitar el seguimiento de los planos posteriores, agregue las siguientes secciones al archivo web.config, bajo la raíz **configuración** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
